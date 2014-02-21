
## Flocking is Declarative

Declarative programming is the source of significant debate and confusion within both the academic literature (Lloyd 2) and the forums of practicing programmers (C2 Wiki). This is due, in part, to a preponderance of vague and partial definitions that fail to provide a coherent model for understanding the approach. Since we specifically characterize Flocking as a declarative framework, it is worth exploring the specifics of what this means to computer musicians, both in principle and in practice.

J.W. Lloyd's informal definition of declarative programming is probably the most helpful starting point for establishing a pragmatic understanding of the idiom. "Declarative programming involves stating _what_ is to be computed but not necessarily _how_ it is to be computed" (1). The emphasis here is on the logical or semantic aspects of computation, rather than on low-level sequencing and control flow. The familiar unit generator abstraction, established in the 1950s by Max Mathews in Music IV (34), represents a partial example of a declarative approach. Unit generators represent a higher-level abstraction for audio signals; they're building blocks that a musician connects together to produce a working instrument that is managed by the synthesis engine. The musician doesn't directly specify the line-by-line sequencing of _how_ a sine wave, for example, is produced. Rather, she specifies the desired signal-generating processes by name and lets the synthesis environment do the rest.

There is, however, a more important aspect to declarative programming than just abstracting away sequential details. Truly declarative programming involves the ability to write programs that are themselves represented in a format that can be processed by other programs as data. Paul Graham describes the declarative nature of Lisp, saying "Lisp has no syntax. You write programs in the parse trees... [that] are fully accessible to your programs. You can write programs that manipulate them... programs that write programs."

The essential characteristic of declarative programming, then, is the unification of "code" with "data," opening up the potential to create software programs that can be understood, manipulated, and transformed by other programs, not just by a compiler. This quality is not just of theoretical interest for musicians. We argue that declarative programming has the potential to bridge the long-standing gap in the computer music world between musicians who write code and those who use graphical patching environments like Max or Pd.

### The Role of JSON

The key to Flocking's declarative approach is JSON, the JavaScript Object Notation format. JSON is a lightweight data interchange format based on a subset of JavaScript that can be parsed and understood in nearly any programming language (Crockford _json.org_). JSON provides two primary data structures that are practically universal across programming languages: 

1. objects consisting of key/value pairs (i.e. dictionaries or associative arrays)
2. ordered lists (i.e. arrays or lists)

Since JSON's syntax and semantics are identical to the Object and Array type literals in JavaScript, JSON is an exceptionally convenient language for representing data in web applications. All of Flocking's musical primitives are expressed as trees of JSON objects.

## Examples of Declarative Synths

## Extensibilty and Compositionality

Where, as mentioned in Wyse & Subramanian, Max and PD provide compositionality by defining patch objects, and SuperCollider and ChucK provide this with functions, Flocking enables exceptional compositionality through "documents"--declarative JSON specifications--and a simple document merging process enabled by the Infusion framework.

## Flocking's Relationship to the Web Audio Ecosystem

<super quick overview of the state of web audio>

It is the opinion of the authors that the current W3C Web Audio API, without the support of additional libraries, is insufficient to support the expressivity required by creative musicians. Many of the limitations of the API are outlined in detail in (Wyse and Subramanian). Web Audio currently provides limited options for web developers who want to create their own custom synthesis algorithms in JavaScript and expect them to perform well. In particular, it is difficult to mix native and JavaScript-based nodes in the same signal graph without imposing latency and synchronization issues.

Flocking predates the first Web Audio API implementation, and was architected specifically for an environment where web developers could contribute their own first-class signal processing implementations. As a result of this philosophy, and due to the performance and "developer experience" issues of the current Web Audio specific, Flocking uses only small parts of the API. Instead, it takes full control of the sample-generation process and provides musicians with an open palette of signal-generating building blocks that can be used to assemble sophisticated digital instruments.

Several other libraries also take a similar "all JavaScript" approach. _Gibberish_ (Roberts) and _CoffeeCollider_ (Mohayonao) are two of the more prominent alternatives to Flocking. CoffeeCollider attempts to replicate the SuperCollider language as closely as possible using the CoffeeScript programming language (Ashkenas), while Gibberish takes a more traditional object-oriented approach. While these environments each offer their own unique features, none have attempted to stray far from the conventional models established by existing music programming environments.

Flocking, too, has taken architectural inspiration from a variety of existing music programming environments, particularly the design of the SuperCollider 3 synthesis server. Flocking shares with scsynth a simple "functions and state" architecture for unit generators, as well as a strict (conceptual) separation between the strict realtime constraints of the signal-processing world and the more dynamic and event-driven application space (McCartney 64).

Flocking, however, takes a markedly different approach to the semantic and constructional issues of digital instrument-building than SuperCollider or other imperative object-oriented environments. In several key ways, it is inspired by the web architecture itself, particularly the emphasis on declarative programming (Berners-Lee x) and stable, publicly addressable state (Fielding x).

### Web Audio and Performance

Much has been written about the current performance issues related to the current generation of JavaScript runtimes generally (lack of deterministic garbage collection) and the Web Audio API specifically (the requirement for ScriptProcessorNodes to run on the main browser thread) [Wyse and Subramanian] [Roberts]. It appears likely that the performance characteristics of the JavaScript language in general will continue to improve as the browser performance wars continue to rage between Mozilla, Google, and Apple. In addition, Web Worker-based strategies for sample generation are currently being discussed for inclusion in the Web Audio API specification (https://github.com/WebAudio/web-audio-api/issues/113).

In the interim, many claims have been made about the relative performance merits of various optimization strategies used in toolkits such as Gibberish (Roberts x). Most of these claims, however, focus on micro-benchmarks that measure only the cost of small-scale operations in isolation, rather than taking into account the performance of real-world signal processing algorithms.

In contrast to the risks of micro-benchmarking and premature optimization, the approach we have taken in Flocking is to focus on an architecture and framework that can serve as a flexible, long-term foundation on which to continually evolve new features and improved performance. Significant effort has been invested into developing automated unit and performance tests for Flocking that measure the real-world costs of its approach.

With just-in-time compilers such as Google's V8 and Mozilla's IonMonkey, we believe that real-world performance is best achieved by using simple algorithms that represent stable "hot loops" that can be quickly and permanently be compiled into machine code by the runtime. The risk of micro-optimization attempts such as the code-generation techniques promoted by Gibberish is "lumpy" (i.e. indeterminate) real-world performance. This risk is caused by the JavaScript runtime having to re-trace and recompile code that is dynamically generated and evaluated whenever the signal generating graph changes. Flocking avoids this risk while maintaining competitive performance by using a simple ordered list scheme for traversing and evaluating unit generators.

# References

C2 Wiki. _Declarative Programming_. Web. http://c2.com/cgi/wiki?DeclarativeProgramming. Accessed January 28, 2014.

Crockford, Douglas. _Introducing JSON_. Web. http://www.json.org/ Accessed January 30, 2014.

Graham, Paul. Beating the Averages. Web. http://www.paulgraham.com/avg.html Accessed January 26, 2014.