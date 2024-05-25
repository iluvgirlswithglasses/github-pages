---
layout: page
highlighter: rouge
title: C++ Programming Style in my Repositories
---

> The term "this repository" refers to all my C++ projects created from March 1st, 2024, onward. This use of ambiguous language exists because this document was originally written exclusively for only one project.

This repository strictly adheres to the [C-Kernel style](https://www.kernel.org/doc/html/v4.10/process/coding-style.html), but with a few modifications and additions.

# 1. Modifications

This section addresses the modifications this repository takes towards the C-Kernel style.

## 1.1. Indentation

An indentation is 4 characters width; and indent using spaces, not tabs. My codebases before 2024-05-24 use tabs and f that.

Only indent what needs indentation. E.g. Allign the `namespace` keyword with its direct subordinates in the same column; do not waste the first indentation level here.

*"If you need more than 3 levels of indentation, you’re screwed anyway, and should fix your program."* does not apply in my repositories.

Sometimes it is just so much more rational to use one additional indentation than having to use exotic labels, or declaring redundant functions/lambdas. This repository strictly limits the use of indentations; it DOES NOT, however, limit how many.

## 1.2. Pointers

When declaring pointer data or a function that returns a pointer type, the preferred use of `*` is adjacent to the data type and not adjacent to the variable or the function name. This is in contrast to the C Kernel Style. The following examples show how this repository declares a pointer:

{% highlight c++ %}
char* linux_banner;
uint64_t memparse(char* ptr, char** retptr);
char* match_strdup(substring_t* s);
{% endhighlight %}

Rationale: The pointer is a type of it own. Using `char* ptr` implies that `ptr` has type `char*`, and this is nice.

# 2. Additions

This section addresses the additional rules of the coding style.

## 2.1. File Naming

Files and directories are named in `snake_case`. Moreover, library files must be named with nouns, while executable files must start with a verb.

## 2.2. Header Files' Definition

The header definition should specify the relative path of the header within the project's source directory. In this specification, slashes are replaced with two consecutive underscores "\_\_", while other separators are replaced with a single underscore "_". For instance, a header file with path `<project>/lib/ibface/face_detector.h` must include the following definition:


{% highlight c++ %}
#ifndef LIB__IBFACE__FACE_DETECTOR_H
#define LIB__IBFACE__FACE_DETECTOR_H

[...]

#endif  // LIB__IBFACE__FACE_DETECTOR_H
{% endhighlight %}

## 2.3. Structs, Functions, and Variable Naming

In all honesty, one of the most practical philosophies out of the C-Kernel style is how things are named:

> C is a Spartan language, and so should your naming be. Unlike Modula-2 and Pascal programmers, C programmers do not use cute names like `ThisVariableIsATemporaryCounter`. A C programmer would call that variable `tmp`, which is much easier to write, and not the least more difficult to understand.
>
> HOWEVER, while mixed-case names are frowned upon, descriptive names for global variables are a must. To call a global function `foo` is a shooting offense. GLOBAL variables (to be used only if you really need them) need to have descriptive names, as do global functions. If you have a function that counts the number of active users, you should call that `count_active_users()` or similar, you should not call it `cntusr()`.
>
> Encoding the type of a function into the name (so-called Hungarian notation) is brain damaged -- the compiler knows the types anyway and can check those, and it only confuses the programmer. No wonder MicroSoft makes buggy programs.
>
> LOCAL variable names should be short, and to the point. If you have some random integer loop counter, it should probably be called `i`. Calling it `loop_counter` is non-productive, if there is no chance of it being mis-understood. Similarly, `tmp` can be just about any type of variable that is used to hold a temporary value.

With this philosophy in mind, here goes.

User-defined structures are named in `PascalCase`. Public constants are also named in `PascalCase`. Their names must be as descriptive as possible. Examples:

{% highlight c++ %}
struct Param
{
public:
    static constexpr bool
        AllowDataClustering = false,
        AllowQuickSkip = false,
        AllowQuestionableStatusCleanse = true;
    [...]
}
{% endhighlight %}

However, for alias types, use `snake_case` with a `_t` suffix:

{% highlight c++ %}
class Orient
{
public:
    using input_t = dlib::matrix<uint8_t, ImgH, ImgW>;
    using ori_t = std::complex<float>;
    using gra_img_t = dlib::matrix<float, ImgH, ImgW>;
    using ori_img_t = dlib::matrix<ori_t, MapH, MapW>;

    [...]
};
{% endhighlight %}

Class methods are named in `snake_case`, and the names should be descriptive too:

{% highlight c++ %}
static int16_t angle(int16_t a, int16_t b);
static int16_t clockwise_angle(int16_t a, int16_t b);
int32_t foo(int32_t bar);
{% endhighlight %}

As for variables, including local variables and class members, they are named in `snake_case`, and it is encouraged to encode their name so as to reduce length. There are only a few rules to follow if you plan to encode a name:

- Comment the meaning right next to the declaration.
- Be consistent. Just don't make a new encoding style for every variable.

For references, these are variable names that are encoded by their consonants:

{% highlight c++ %}
class Matcher
{
    [...]

    TripletPairs trps;  // TRiplet PairS
    MinutiaPairs mnps;  // MiNutia PairS
    MinutiaDupes pdps;  // Probe's DuPeS
    MinutiaDupes cdps;  // Candidate's DuPeS

};
{% endhighlight %}

## 2.4. Brackets and Parentheses

The C-Kernel's convention for these is nice, but it doesn't cover a specific case: "What do I do if the function's parameters list is ungodly long and I need linebreaks?"

This repository does the following for this kind of prototypes:

{% highlight c++ %}
int32_t function(
    const image_t& src_image,
    const std::vector<dlib::rectangle>& truth_labels,
    int something_funny
) const;
{% endhighlight %}

and this for their implementation:

{% highlight c++ %}
int32_t function(
    const image_t& src_image,
    const std::vector<dlib::rectangle>& truth_labels,
    int something_funny
) const
{
    // some codes
}
{% endhighlight %}

## 2.5. Comments

There are 4 types of comments in this repository.

### 2.5.1. Documentation Comment for Struct, Classes, and Functions

For structs and classes, what they represent and their notices shall be commented like this (please pay attention to the indentation and the linebreaks):

{% highlight c++ %}
/*!
WHAT THIS OBJECT REPRESENTS:

    What defines Undead Corporation? Think potent guitar riffs, rapid-
    fire drum beats, and vocals that hit scream-core levels of
    intensity. These guys are the wildest rock band I've encountered
    in my five years of navigating the metal scene. While they also
    focus on Touhou re-arrangements like the previous bands, Undead
    Corporation takes a different approach -- they rip the melodies
    off and tune them into a chaotic symphony.

NOTICES:

    Yet, behind the chaos lies an ensemble of undeniably talented
    musicians. Crafting madness into something exciting and enjoyable
    is an exceptional talent in itself.

!*/
struct TheStruct
{
    [...]
};
{% endhighlight %}

Unlike structs and classes, functions have their documentation comment written **after** their **prototype declaration**. Rationale: You need to know the function's name, its return type, its parameters, its additional attributes (like `const`), et cetera- before learning what it does.

And, please note never to write a function's documentation comment in the implementation. Do so in the **prototype declaration** only. Following is a good example:

{% highlight c++ %}
int32_t* detect(IbFace::FaceDetector*, const char* fimg);
/*!
REQUIRES:

    - A pointer to a face detector object
    - A path to an image

RETURNS:

    An array `a[]` where:
    - a[0]: The number of detected rectangles
    - a[1 + 4i + 0]: Top coord of the i-th rectangle
    - a[1 + 4i + 1]: Left coord
    - a[1 + 4i + 2]: Bottom coord
    - a[1 + 4i + 3]: Right coord

!*/
{% endhighlight %}

### 2.5.2. Notice Comment

These are mostly used for denoting the purpose/idea/whatever of a code segment, and are written before that segment. There are no significant rules for these comments. For example:

{% highlight c++ %}
// third condition
// abcxyz (this kind of comment can be multi-line)
const bool rd = [&]() {
    // t2 - t1 + t3, always in range [0:2pi)
    uint8_t t = static_cast<uint8_t>(theta + p2.probe()->a());
    return FMath::angle(t, p2.candi()->a()) <= Param::AngleTolerance;
}();
{% endhighlight %}

### 2.5.3. Inline Comment

These are mostly used for either:

1. Explaining the name of a variable
2. Noting an alternative expression for a dirty trick
3. Something else that's equally awkward

Same as the notice comments, these don't have strict rules. Some examples for them are:

{% highlight c++ %}
MinutiaDupes pdps = something();    // 'pdps' means 'Probe DuPeS'
memset(&pdps[0], 0, probe.size());  // == sizeof(pdps[0]) * pdps.size()
{% endhighlight %}

### 2.5.4. Segment Separator

These comments are used for separating different regions of a file. There are three levels of separators:

Level 1:

{% highlight c++ %}
/**
 * @! Section 5:	M3GL Algorithm + Upgrades
 *
 * Returns the number of matching minutia pairs
 * You may write multi-line notes here, or you may not
 *
 * */
[... code goes here]
{% endhighlight %}

Level 2:

{% highlight c++ %}
/* @! Procedure 5.1.2 */
// notice comments, if necessary
[... code goes here]
{% endhighlight %}

And, level 3:
{% highlight c++ %}
// a label with a fixed length bar ----------------------------------
// also notice comments if necessary
[... code goes here]
{% endhighlight %}

# 3. Shared Library's API

## 3.1. CSharp

For exported functions of the CSharp (C\#) API:

1. The `extern` decorator must be written one line above the function.
2. Each component of a function name must be in `PascalCase`, and these components are separated from each other by an underscore.
3. The function name must be a verb. If it consists of a noun followed by a verb, it signifies that an object (denoted by the noun) will perform the action (denoted by the verb) upon function call.

Following are some examples:

{% highlight c++ %}
extern "C" __declspec(dllexport)
IbFace::FaceDetector* Construct_FaceDetector(const char* fnet);

extern "C" __declspec(dllexport)
void Destruct_FaceDetector(IbFace::FaceDetector*);

extern "C" __declspec(dllexport)
int32_t* FaceDetector_Detect(IbFace::FaceDetector*, const char* fimg);
{% endhighlight %}

