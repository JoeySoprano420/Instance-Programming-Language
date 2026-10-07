# Instance-Programming-Language

INSTANCE — ".ins"

Fully Mature Systems Language Design, Core Syntax, Grammar, and Structural Semantics Specification

Name: Instance
Extension: ".ins"
Class: Native imperative-procedural systems programming language
Compilation: Ahead-of-time native compilation
Frontend: Authoritative EBNF grammar
Backend: Hidden C-based native lowering
Execution priority: Maximum raw runtime throughput
Execution model: Imperative, procedural, instructional, sequence-oriented
Memory model: Instance Virtual Storage — IVS
Error model: C-compatible status/error and "errno" semantics
Structural model: Tapered indentation and instructional sequences
Surface: Minimal, high-level, inference-heavy
Semantics: Native systems-level
Optimization model: Inference-heavy, speculative, dissolving, whole-program-capable
Runtime philosophy: Run only what could not be removed before runtime
Core motto: Define it. Infer it. Reduce it. Run what remains.

---

1. Definition

Instance is an ahead-of-time compiled native programming language designed around one overriding objective:

«Do as much work as possible before execution so the machine performs as little work as possible during execution.»

Instance presents a small, high-level, procedural surface while exposing the semantic freedom necessary for extremely aggressive native optimization.

It is an imperative language.

It is a procedural language.

It is an instructional language.

It is a systems language.

Its source code is intentionally smaller and more abstract than the machine behavior it can describe.

Instance assumes that a programmer generally cares more about:

- what must happen;
- in what sequence;
- under what conditions;
- with what data;
- where information came from;
- what relationships can be established;
- and what ultimately must be produced

than about manually spelling out every intermediate machine mechanism necessary to make it happen.

The compiler fills that gap.

The governing rule of Instance is:

«If it can be defined and placed into logic, it can be optimized.»

Its companion rule is:

«If its behavior cannot be sufficiently defined, constrained, inferred, guarded, or logically established, it may enter undefined behavior.»

Its elimination rule is:

«If it can be folded, merged, dissolved, propagated, specialized, predicted, removed, or made nonexistent, it should be.»

Instance therefore treats runtime work as something that must justify its continued existence.

---

2. Core Philosophy

Instance is founded on six primary principles.

2.1 Define enough

The programmer defines enough of the problem for the compiler to understand its logical boundaries.

The programmer does not necessarily define every implementation detail.

---

2.2 Infer the rest

When information can be derived safely or profitably, the compiler derives it.

This includes:

- types;
- storage behavior;
- temporary lifetimes;
- aliases;
- constants;
- loop extents;
- branch probabilities;
- value ranges;
- call specialization;
- vector width;
- alignment;
- escape behavior;
- allocation strategy;
- calling form;
- data provenance;
- transformation relationships;
- and opportunities for parallel execution.

---

2.3 Optimize definitions, not syntax

Two pieces of source that establish the same logical operation need not produce similar binaries.

The compiler optimizes established meaning, not textual shape.

Source syntax is evidence of semantics.

It is not a mandate for particular machine instructions.

---

2.4 Undefined means optimizer freedom

Undefined behavior is not primarily an error-recovery mechanism.

It is an optimization boundary.

When execution enters behavior for which Instance establishes no required semantics, the implementation owes that execution no particular result.

This permits aggressive assumptions in performance-critical code.

---

2.5 Storage exists when needed

Storage is not assumed to physically exist merely because its possible extent has been described.

Instance separates:

logical extent

from:

virtual address-space reservation

from:

physical commitment.

Large storage objects can therefore describe substantial addressable ranges while consuming physical backing only when required.

---

2.6 Runtime is the residue

The Instance compiler attempts to make compilation absorb everything it reasonably can.

Runtime execution is therefore considered the residue of compilation:

«the operations that could not be proven unnecessary.»

---

3. Instance Motto

The canonical motto is:

«INSTANCE — Define it. Infer it. Reduce it. Run what remains.»

A secondary formulation is:

«Logic first. Machine last. Runtime only when necessary.»

---

4. Governing Syntax Principle

Instance syntax exists primarily to express:

instruction
relationship
sequence
constraint
source
transformation
control
storage
result

For example:

source packet from receive(socket)

derive decoded = decode(packet)

when decoded.valid
    decoded -> normalize -> classify -> dispatch

The programmer expresses the meaningful flow.

The programmer does not normally interact with:

ASTs
SSA
compiler-generated IR
generated C
temporary backend symbols
optimizer passes
object files
linker staging
backend helper functions

Those are implementation machinery.

The visible compilation model remains:

.ins source
      ↓
native executable

---

5. Execution Character

Instance code reads like an ordered collection of instructions.

Example:

process main() -> i32
    count := receive_count()

    when count == 0
        give 0

    source data from acquire(count)

    each item in data
        transform(item)

    emit(data)

    give 0

The programmer sees:

receive
  ↓
evaluate
  ↓
acquire
  ↓
transform
  ↓
emit

The compiler may instead see:

constant propagation
        ↓
allocation sinking
        ↓
loop fusion
        ↓
SIMD transformation
        ↓
dead-store removal
        ↓
direct system write

Instance deliberately separates these viewpoints.

---

6. Processes, Tasks, Nodes, and Sequences

Instance does not use "function" or "routine" as its primary user-facing terminology.

Its principal executable abstractions are:

process
task
node
sequence

These constructs do not inherently imply operating-system processes or threads.

They describe semantic units of work.

---

7. Processes

A "process" represents imperative procedural behavior.

Processes are particularly suitable for:

- command sequences;
- state mutation;
- system interaction;
- device operations;
- orchestration;
- mutation-heavy work;
- application entry points;
- resource control;
- and ordered execution.

Example:

process clear(byte* memory, size count)
    each i in 0..<count
        memory[i] = 0

A process may return a result:

process startup() -> i32
    initialize()
    give 0

Processes may have externally observable side effects.

---

8. Tasks

A "task" describes a logically bounded unit of work, usually result-oriented.

task square(i64 value) -> i64
    give value * value

Tasks carry stronger optimization expectations than general processes.

Where legal, a task may be:

- inlined;
- cloned;
- specialized;
- vectorized;
- duplicated;
- reordered;
- fused;
- evaluated during compilation;
- specialized per caller;
- parallelized;
- memoized where semantics allow;
- or erased completely.

A task does not inherently create a thread.

Example:

task combine(f32 a, f32 b) -> f32
    give a + b

If every call receives known values, "combine" may never exist in the emitted executable.

---

9. Nodes

A "node" is a named computation stage intended especially for:

- dataflow;
- sequencing;
- branch routing;
- graph construction;
- pipeline fusion;
- transformation chains;
- semantic decomposition.

Example:

node normalize(f32 value) -> f32
    give value / 255.0

Another:

node classify(Packet packet) -> Class
    ...

A node is not inherently a runtime object.

A node does not require:

allocation
virtual dispatch
thread creation
graph object construction
runtime metadata

Its primary purpose is to expose a meaningful computational boundary to the compiler.

That boundary may later disappear completely.

---

10. Sequences

A "sequence" represents a reusable, explicitly related chain of work.

sequence prepare(Packet packet) -> Packet
    result := packet -> decode -> normalize -> validate
    give result

Another example:

sequence transform(byte[] data) -> byte[]
    result := data -> decode -> filter -> compress
    give result

Sequences are especially friendly to:

- fusion;
- inter-stage propagation;
- storage elimination;
- specialization;
- vectorization;
- branch elimination;
- pipeline dissolution.

A sequence exists because its relationship is meaningful.

Its stage boundaries do not have to survive runtime.

---

11. Tapered Structure

Instance uses significant indentation.

A structural indentation level is exactly:

4 spaces

Tabs are not legal indentation characters.

Example:

process main() -> i32
    when ready
        start()

        when accelerated
            engage()

    give 0

The visual narrowing of nested instructions is called tapering.

There are no ordinary structural braces.

This:

when condition
    work()

is complete.

There is no required:

}

terminator.

A block exists because indentation establishes it.

Conceptually:

process
    instruction
    condition
        narrower instruction
        narrower instruction
    instruction

The source therefore resembles an instruction hierarchy rather than a collection of brace-delimited containers.

---

12. Logical Lines

A newline terminates an ordinary instruction unless the expression remains syntactically incomplete because it occurs inside:

( )
[ ]
< >

or inside another explicitly continued construct.

Example:

result := calculate(
    first,
    second,
    third
)

Indentation used solely to continue an expression inside an open delimiter is not structural indentation.

---

13. Canonical Formatting

Canonical Instance formatting follows these rules:

- exactly 4 spaces per structural level;
- tabs are prohibited;
- one ordinary instruction per logical line;
- binary operators use one surrounding space;
- no space appears between callable name and "(";
- one space follows commas;
- a block header ends at newline;
- semicolons are not structural terminators;
- braces are not structural delimiters;
- blank lines may separate conceptual phases;
- continuation indentation inside delimiters is nonstructural;
- pointer member access is written without spaces as "pointer->member";
- sequencing uses spaced arrows such as "value -> normalize".

Example:

process calculate(i32 first, i32 second) -> i32
    derive total = first + second

    when total > 100
        give 100

    give total

---

14. Comments

Single-line comment:

// comment

Block comment:

/*
    comment
*/

Comments do not participate in executable semantics.

They are among the few pieces of Instance source that may be discarded without contributing semantic information.

---

15. Identifiers

Identifiers begin with a letter or underscore.

Valid:

count
_count
packet_index
ParseHeader
node7

Invalid:

7node
packet-index
packet index

Identifiers are case-sensitive.

Therefore:

value
Value
VALUE

are distinct identifiers.

---

16. Reserved Keywords

The core reserved words are:

process
task
node
sequence

structure
union
enum
tuple

foreign
c

const
global

source
derive
storage

when
else
unless
branch
case
otherwise

while
until
repeat
each
in
by

map

parallel
spawn
await
detach

assume
check
fail

give
break
continue
stop
skip

commit
decommit
release

format
with
from

as

true
false
null

and
or
not

sizeof
alignof

error
void

Primitive type names are also reserved.

---

17. Fundamental Types

Integer types:

i8
i16
i32
i64
i128

u8
u16
u32
u64
u128

Floating-point types:

f16
f32
f64
f128

Core machine types:

bool
byte
char
size
addr
int
void
error
text

"size" is the preferred native unsigned object-size type.

"addr" is an uninterpreted native machine address.

"int" is a native C-ABI-compatible signed integer type.

Explicit-width types such as "i32" and "i64" are preferred where stable representation matters.

"error" is a C-compatible integer status representation.

"text" is an immutable UTF-8 view.

A "text" value does not inherently imply heap ownership.

---

18. Type Construction

Instance uses:

«type before name»

Example:

i32 count
f64 distance
byte value

Type modifiers belong to the type itself.

Canonical examples:

byte* data
i32& reference
i32[64] values
i32[] view

Instance does not use C's declarator-placement rules.

---

19. Pointer Types

Pointer:

i32* pointer

Pointer to pointer:

i32** pointer

Pointer to byte:

byte* memory

Pointers may contain "null".

Dereference:

value := *pointer

Address-of:

pointer := &value

Pointer member access:

object->field

Pointer arithmetic follows native systems semantics.

Pointer operations outside an established valid addressable object or storage region are undefined.

---

20. References

Reference:

i32& reference

A reference represents an established alias to an existing object.

A valid reference cannot be null.

Example:

task increment(i32& value)
    value += 1

References do not inherently create ownership.

---

21. Fixed Arrays

Canonical fixed-size array:

i32[64] values

Two-dimensional array:

i32[32][64] matrix

Array of pointers:

i32*[64] pointers

Pointer to fixed array:

i32[64]* block

The ordering is intentional.

i32*[64]

means:

«array of 64 pointers to "i32"»

while:

i32[64]*

means:

«pointer to an array of 64 "i32" values.»

Ordinary fixed-array extents are compile-time expressions.

Runtime-sized, demand-backed regions use IVS "storage".

---

22. Views and Slices

Open-length view:

i32[] values

A slice/view contains enough information for the compiler to establish its accessible extent.

It does not inherently own storage.

Example:

process normalize(f32[] values)
    each value in values
        value = value / 255.0

Arrays, slices, and IVS regions provide standard intrinsic observations where semantically available:

.count
.ptr

Example:

write(1, data.ptr, data.count)

---

23. Tuples

Instance supports tuples as lightweight product values.

Tuple type:

tuple<i32, f32> pair

Tuple literal:

pair := (10, 3.5)

A tuple does not imply object allocation.

It may be:

- register-packed;
- scalar-replaced;
- decomposed;
- flattened;
- returned through ABI registers;
- or dissolved entirely.

---

24. Structures

Structure declaration:

structure Point
    f32 x
    f32 y

Usage:

Point position

position.x = 10.0
position.y = 20.0

Initialization:

Point position = Point(10.0, 20.0)

Structures have native representation semantics.

The compiler may alter, flatten, scalar-replace, or dissolve their representation unless representation becomes observable through:

foreign ABI
raw memory access
address use
explicit layout contract
binary serialization
device interface

---

25. Unions

Union:

union Number
    i64 integer
    f64 floating

Only interpretations established by valid program logic are defined.

Reading a union representation that conflicts with the established active interpretation may invoke undefined behavior.

---

26. Enumerations

Enumeration:

enum State
    idle
    running
    stopped
    failed

Explicit values:

enum State
    idle = 0
    running = 1
    stopped = 4
    failed = 255

Usage:

State state = State.running

An invalid bit pattern interpreted as an enum is undefined unless passed through an operation that explicitly validates that conversion.

---

27. Ordinary Variables

Inferred declaration:

count := 50

Explicit declaration:

i32 count = 50

Uninitialized declaration:

i32 count

Reading an uninitialized value is undefined.

The compiler is not required to initialize ordinary storage defensively.

---

28. Constants

const i32 maximum = 100

A constant is immutable after establishment.

Compile-time-evaluable constants should be consumed by optimization whenever profitable.

---

29. Globals

global i64 requests = 0

Global storage has program lifetime unless analysis proves the global can be:

- internalized;
- localized;
- folded;
- duplicated;
- specialized;
- or removed.

---

30. Derivation

Instance provides:

derive

A derived binding describes a value logically obtained from existing information.

Example:

derive doubled = value * 2

Explicitly typed:

derive i64 doubled = value * 2

A derived binding has no required independent storage identity.

Therefore:

derive area = width * height

does not require:

load width
load height
multiply
store area
reload area

The compiler may substitute the relationship wherever necessary.

Derived values are strong candidates for:

- constant folding;
- expression substitution;
- common-subexpression elimination;
- register propagation;
- vector fusion;
- scalar replacement;
- complete disappearance.

If the program requires an address:

pointer := &area

the compiler materializes storage only if defined semantics require an addressable object.

---

31. Sourcing

Instance provides:

source

A source binding indicates that a value originates from another operation, interface, stream, provider, system, or provenance boundary.

Inferred source:

source packet from receive(socket)

Typed source:

source byte[] packet from receive(socket)

Another example:

source config from load_configuration(path)

"source" differs from ordinary assignment because it establishes provenance.

The compiler may use provenance to reason about:

- alias relationships;
- external mutation;
- memory origin;
- lifetime;
- dataflow;
- I/O dependency;
- branch likelihood;
- storage placement;
- synchronization;
- escape behavior.

Example:

source packet from network.receive(socket)
derive header = parse_header(packet)

"packet" is explicitly an origin.

"header" is explicitly a derivation.

---

32. Instructional Control Flow

Primary control instructions are:

when
else
unless

branch
case
otherwise

while
until
repeat
each

break
continue
give
fail
stop
skip

Example:

when temperature > limit
    shutdown()
else
    continue_work()

---

33. When

when count > 0
    execute()

Chaining:

when value > 100
    high()
else when value > 10
    medium()
else
    low()

---

34. Unless

unless ready
    initialize()

Equivalent logical condition:

not ready

An "else" block is legal:

unless ready
    initialize()
else
    execute()

---

35. Branching

Multi-way branch:

branch state
    case State.idle
        initialize()

    case State.running
        execute()

    case State.failed
        recover()

    otherwise
        stop

The branch subject is evaluated once.

---

36. Multiple Branch Selectors

branch value
    case 0, 1, 2
        low()

    case 3..10
        medium()

    otherwise
        high()

---

37. Guarded Cases

branch packet.kind
    case Data when packet.valid
        process_data()

    case Control
        process_control()

    otherwise
        reject()

A case guard is evaluated only after its selector matches.

---

38. Branch Optimization

A branch may become:

jump table
comparison chain
bit test
lookup
predication
compile-time constant
direct node route
nothing

depending on what the compiler establishes.

---

39. While

while active
    update()

---

40. Until

until ready
    poll()

Conceptually:

while not ready

---

41. Repeat

repeat 8
    tick()

Known repetition counts are strong candidates for:

- unrolling;
- folding;
- vectorization;
- deletion.

---

42. Each

each item in values
    consume(item)

Index and value:

each index, item in values
    output[index] = transform(item)

Range iteration:

each i in 0..<count
    output[i] = input[i]

"each" provides unusually strong optimizer freedom.

Depending on dependencies, it may become:

- an ordinary scalar loop;
- an unrolled sequence;
- SIMD;
- vector instructions;
- fused loops;
- parallel partitions;
- compile-time evaluation;
- or nothing.

---

43. Ranges

Inclusive:

0..10

means:

0 through 10

Half-open:

0..<10

means:

0 through 9

Stepped:

0..<100 by 4

Descending:

100..0 by -1

A zero step is undefined.

A range whose step cannot progress toward its terminating boundary yields no iterations unless a surrounding construct explicitly defines another interpretation.

Known ranges are valuable because they expose bounds directly to optimization.

---

44. Sequencing

Instructions execute sequentially unless established semantics permit reordering.

Example:

read(input)
decode(input)
transform(input)
write(output)

The compiler may reorder operations only where all defined observable behavior remains valid.

Instance therefore preserves instructional readability without treating source order as an unnecessary optimization barrier.

---

45. Sequence Operator

The sequencing operator is:

->

Example:

packet -> decode -> normalize -> classify

Conceptually:

packet
  ↓
decode
  ↓
normalize
  ↓
classify

The result of the previous stage becomes the implicit first input of the following stage.

Therefore:

value -> scale(4) -> clamp(0, 255)

is logically comparable to:

clamp(scale(value, 4), 0, 255)

but it exposes a sequencing graph instead of forcing nested-call syntax.

---

46. Sequence Dependencies

data -> parse -> normalize -> encode

establishes:

parse before normalized result
normalize before encoded result

It does not establish separate runtime calls.

The complete chain may become one loop or one machine-level kernel.

---

47. Mapping

Instance provides first-class mapping.

mapped := map item in values => transform(item)

Example:

doubled := map value in values => value * 2

Typed destination:

i32[] doubled = map value in values => value * 2

Mapping does not inherently allocate a second collection.

The compiler may:

- materialize output;
- stream results;
- fuse consumers;
- vectorize;
- parallelize;
- scalarize;
- precompute;
- eliminate the map.

Example:

result := map item in values => normalize(item)
consume(result)

may become one fused traversal without a separately materialized "result".

---

48. Mapping With Index

result := map index, item in values => item * weights[index]

---

49. Mapping Over Ranges

squares := map i in 0..<100 => i * i

If sufficiently known and profitable, this may be evaluated during compilation.

---

50. Formatting

Instance has a built-in formatting expression.

message := format "count={}" with count

Multiple values:

message := format "x={} y={} total={}" with x, y, total

Formatting produces "text".

Storage is not prescribed.

The compiler may use:

- compile-time constant text;
- stack storage;
- existing destination storage;
- temporary arena storage;
- IVS;
- direct output fusion.

Example:

const i32 version = 4
message := format "Instance version {}" with version

may become a constant string.

No runtime formatter is required if the result is known at compile time.

---

51. Definitions Versus Mechanisms

Instance distinguishes:

Definition

What must logically be true.

Mechanism

How the machine eventually produces that truth.

Example:

each x in values
    x *= 2

does not require one scalar multiply instruction per element.

It establishes the result.

The compiler may produce:

- SIMD multiplication;
- wider vector operations;
- fused operations;
- target-specific accelerator lowering;
- precomputation;
- or no multiplication if the result can be determined without it.

---

52. Explicit Assumptions

The programmer may contribute semantic truths:

assume pointer != null
assume count > 0
assume count % 16 == 0
assume source != destination

"assume" does not inherently generate a runtime check.

It establishes compiler knowledge.

Thus:

assume count > 0

means:

«Optimize following execution as though "count > 0" is always true.»

If execution reaches the assumption while the proposition is false, subsequent behavior is undefined.

---

53. Aliasing

Pointers and references may alias unless the compiler proves stronger relationships.

The canonical programmer-directed no-alias form is:

assume noalias(destination, source)

"noalias" is a compiler intrinsic.

Example:

process blend(f32[] output, f32[] left, f32[] right)
    assume noalias(output, left)
    assume noalias(output, right)

    each i in 0..<output.count
        output[i] = left[i] + right[i]

These assumptions may unlock:

- SIMD;
- load hoisting;
- store forwarding;
- loop reordering;
- block copying;
- vector loads;
- parallelization.

Violating an established "noalias" assumption invokes undefined behavior.

---

54. Definedness Gradient

Instance behavior exists on a definedness gradient.

Level 1 — Proven

Behavior is completely established.

compiler knows

Maximum valid optimization is permitted.

---

Level 2 — Constrained

Behavior is not fully known, but its boundaries are sufficiently established.

compiler knows enough

---

Level 3 — Assumed

The programmer or compiler operates under an established assumption.

Example:

assume count > 0

---

Level 4 — Speculative

The compiler constructs an optimized hypothesis protected by proof, guard, fallback, or an already-valid undefined-behavior boundary.

---

Level 5 — Undefined

The language establishes no required result for that execution.

At this level the violated semantic requirement no longer constrains optimization.

---

55. Undefined Behavior

Instance deliberately preserves undefined behavior as a performance tool.

Typical UB domains include:

- invalid pointer dereference;
- prohibited alias violation;
- access beyond established storage;
- use of expired references;
- false "assume" conditions;
- impossible enum states;
- invalid shift widths;
- unsupported arithmetic conditions;
- misuse of foreign interfaces;
- unsynchronized data races;
- access through released storage;
- malformed raw-memory interpretations;
- invalid lifetime assumptions;
- reading uninitialized values.

Instance does not attempt to recover automatically from every programmer violation.

Doing so would impose runtime machinery inconsistent with its primary design objective.

---

56. Speculation

The compiler may speculatively optimize likely execution paths.

It may:

- predict common values;
- specialize common call forms;
- isolate rare paths;
- hoist checks;
- build fast paths;
- precompute likely layouts;
- reorder independent operations;
- generate guarded specialized variants.

A speculative transformation must either:

1. be logically proven;
2. have a valid fallback;
3. have a guard;
4. or operate inside semantics where a violated assumption is already undefined.

Speculation does not license arbitrary compiler miscompilation.

---

57. Context-Free Guessing

Instance explicitly permits context-free compiler guessing.

The compiler may form provisional optimization hypotheses from immediately available syntax and local semantic information before full global context is known.

It may initially hypothesize that:

- a pointer is aligned;
- a task does not escape;
- a temporary remains local;
- a branch is cold;
- two regions do not overlap;
- a sequence can fuse.

These are optimization candidates, not automatic truths.

The compiler subsequently attempts to:

- prove them;
- guard them;
- invalidate them;
- or show that a violating case already enters undefined behavior.

---

58. Dynamic Compiler Supercharging

Instance compilation is aggressive and adaptive.

Newly established knowledge may retroactively simplify earlier structures.

Example:

infer value range
      ↓
prove condition constant
      ↓
remove branch
      ↓
discover object no longer escapes
      ↓
remove allocation
      ↓
inline producer
      ↓
fold producer
      ↓
remove entire task

Compilation therefore behaves less like a one-pass translation and more like an iterative semantic reduction engine.

---

59. Inference

Instance aggressively infers unspecified information.

Example:

amount := 42

The compiler may infer:

- integer type;
- practical width;
- range;
- mutability;
- storage location;
- lifetime;
- constant status;
- escape behavior;
- whether storage is needed at all.

If "amount" never needs an address, it may never receive one.

If it becomes a compile-time constant, it may never exist at runtime.

---

60. Abstraction Dissolution

High-level abstractions are expected to disappear whenever possible.

An abstraction may be:

- flattened;
- specialized;
- decomposed;
- merged;
- constant-folded;
- scalar-replaced;
- inlined;
- fused;
- or completely dissolved.

The ideal Instance zero-cost abstraction is not merely cheap.

It is absent when its existence no longer contributes observable behavior.

---

61. Instance Virtual Storage

Instance's demand-backed virtual-memory system is:

IVS — Instance Virtual Storage

IVS separates:

logical extent
virtual reservation
physical commitment

A storage region may describe a very large address range without immediately consuming equivalent physical memory.

Canonical declaration:

storage byte[count] buffer

Example:

storage State[maximum_entities] state

A "storage" declaration establishes:

- logical extent;
- virtual addressable extent;
- compiler-managed backing policy.

It does not require immediate full physical commitment.

---

62. Storage As Needed

The governing IVS rule is:

«Storage follows demonstrated need.»

A 4 GiB logical arena that touches only 30 MiB should not inherently require 4 GiB of physical backing.

IVS is especially useful for:

- sparse tables;
- databases;
- compilers;
- asset systems;
- simulation;
- games;
- memory arenas;
- scientific workloads;
- servers;
- caches;
- virtual machines;
- sparse matrices;
- procedural generation.

---

63. Initialized IVS Storage

storage i32[count] values = 0

Initialization may occur:

- eagerly;
- lazily;
- on page commitment;
- through zero-page mapping;
- through optimizer synthesis;
- or not at runtime at all where the result can be proven.

Defined observable behavior must remain equivalent.

---

64. IVS Storage Access

buffer[index] = value

Access outside the established logical extent is undefined.

The compiler is not required to insert bounds checks.

---

65. Commit

Programmers may explicitly request physical backing.

commit buffer

Partial region:

commit buffer[0..<4096]

"commit" affects storage strategy, not logical extent.

---

66. Decommit

decommit buffer[4096..<8192]

This requests removal of physical backing where supported.

For portable predictable behavior, explicitly write valid state before subsequently reading a region after decommit/recommit.

The implementation may exploit platform zero-page or equivalent facilities where semantics permit.

---

67. Release

release buffer

After release, use of:

- the storage binding;
- pointers into the region;
- references into it;
- slices into it

is undefined unless an independently valid object was established elsewhere.

---

68. Ordinary Storage Versus IVS

Ordinary:

i32 value
i32[64] local

Demand-backed:

storage i32[count] large_region

The compiler may internally use virtual-memory techniques for ordinary objects as well.

The "storage" keyword specifically tells Instance:

«This represents an addressable storage region whose physical backing may follow demand.»

---

69. Automatic Storage Elimination

This:

storage i32[1000000] values

does not force one million integers to survive as physical runtime storage.

If analysis establishes that only:

values[0]
values[4]
values[20]

matter, the compiler may represent only the observably necessary subset.

---

70. Stack Versus Virtual Storage

Small, bounded, proven-local objects usually prefer:

register
   ↓
stack

Large, elastic, sparse, or uncertain objects may prefer:

virtual reservation
        ↓
demand commitment

The compiler normally chooses automatically.

Explicit IVS is used when the programmer intends demand-backed addressable storage.

---

71. Hot-Path Storage Optimization

Demand commitment must not become a source of needless runtime overhead.

Instance may:

- precommit predicted hot windows;
- batch commitment;
- hoist commitment outside loops;
- retain frequently reused pages;
- decommit only when profitable;
- use larger pages when beneficial;
- align regions to machine boundaries;
- merge adjacent storage requests.

---

72. IVS Is Not Garbage Collection

IVS is not a tracing garbage collector.

Instance does not require:

- heap tracing;
- stop-the-world collection;
- moving objects;
- mandatory reference counting;
- hidden object headers.

Storage lifetime may be established using:

- lexical lifetime;
- escape analysis;
- process lifetime;
- region analysis;
- explicit release;
- ownership inference;
- whole-program analysis;
- operating-system virtual-memory facilities.

---

73. No Mandatory Garbage Collector

Instance has no mandatory GC.

This supports predictable systems behavior suitable for:

- engines;
- servers;
- embedded applications;
- latency-sensitive code;
- realtime-adjacent systems;
- native libraries;
- system components.

---

74. Error Philosophy

Errors are C-based.

Instance has no mandatory exception system and no mandatory stack unwinder.

Core error mechanisms include:

- integer status values;
- nulls;
- sentinels;
- C-compatible "errno";
- explicit failure;
- explicit checking;
- explicit propagation.

---

75. Error Type

The built-in:

error

is C-compatible with an integer status representation.

Example:

task open_file(char* path) -> error
    ...

Canonical convention:

0       success
nonzero error/status

where appropriate.

---

76. errno

Instance exposes:

errno

for C-compatible platform error integration.

Example:

when descriptor < 0
    print(errno)

Its implementation may be:

- thread-local;
- platform-runtime supplied;
- compiler intrinsic;
- ABI mapped.

---

77. Fail

Inside an "error"-returning callable:

task validate(i32 count) -> error
    when count < 0
        fail EINVAL

    give 0

Conceptually:

errno = EINVAL
return EINVAL

where compatible with the platform environment.

---

78. Fail With Alternate Return

For C-style APIs returning sentinel values:

fail EINVAL with -1

Example:

task open_resource(text path) -> int
    unless path.valid
        fail EINVAL with -1

    ...

Conceptually:

errno = EINVAL
return -1

No exception object is created.

No stack unwinder is required.

---

79. Check

Error propagation:

check initialize()
check load()
check execute()

Inside an "error"-returning callable:

check initialize()

is logically equivalent to:

error temporary = initialize()

when temporary != 0
    give temporary

The compiler is expected to eliminate unnecessary temporary storage.

---

80. Give

Return from a:

- process;
- task;
- node;
- sequence

with:

give value

Void result:

give

There is no separate canonical "return" keyword.

---

81. Stop

stop

terminates the current process-level operation where legal.

Status:

stop 1

At an application entry point this may map directly to native process termination.

---

82. Break

break

leaves the nearest enclosing loop.

---

83. Continue

continue

advances the nearest loop to its next iteration.

---

84. Skip

skip

is an explicit no-operation.

It can serve as:

- branch placeholder;
- generated-code placeholder;
- deliberately empty body marker.

It normally emits nothing.

---

85. Native C Interoperability

C interoperability is first-class.

Example:

foreign c process puts(char* message) -> int

Another:

foreign c process write(int fd, byte* data, size count) -> int

Variadic:

foreign c process printf(char* pattern, ...) -> int

Instance can naturally interact with:

- operating-system APIs;
- C libraries;
- drivers;
- graphics APIs;
- compression libraries;
- databases;
- network stacks;
- embedded SDKs;
- existing native systems code.

---

86. Foreign Boundaries

A foreign boundary is an optimization barrier only to the extent required by its observable semantics.

Compiler-visible metadata may describe:

- purity;
- aliasing;
- mutation;
- alignment;
- lifetime;
- return behavior;
- failure behavior;
- external memory effects.

This allows foreign operations to participate in optimization more effectively.

---

87. Foreign Calls

result := write(1, data.ptr, data.count)

The compiler performs ABI adaptation internally.

The programmer does not write or maintain generated bridge C.

---

88. Arithmetic

Ordinary Instance arithmetic is native-first.

a := b + c

Core operations include:

+
-
*
/
%

Bitwise:

&
|
^
~
<<
>>

Logical:

and
or
not

Comparison:

==
!=
<
<=
>
>=

Ordinary arithmetic does not impose universal runtime overflow machinery.

Specialized checked or saturating behavior may be supplied by compiler intrinsics or standard-library facilities where explicitly requested.

---

89. Casting

Explicit conversion:

value as i64

Example:

i64 large = small as i64

Unchecked conversions whose requested representation cannot satisfy the semantic value may be implementation-defined or undefined according to the target conversion class.

Validated conversions belong to explicit checked facilities.

---

90. Size and Alignment

sizeof(i64)
sizeof(value)

alignof(i64)

These are compile-time operations where layout is statically established.

---

91. Member Access

Object member:

object.member

Pointer member:

pointer->member

Pointer-member access is written without spaces.

This distinguishes it visually from sequencing:

pointer->member
value -> normalize

The parser additionally resolves the distinction structurally.

---

92. Indexing

values[index]

Multidimensional:

matrix[row][column]

---

93. Calls

calculate(a, b)

A source-level call does not guarantee a runtime call.

It expresses invocation semantics.

The compiler may inline, fold, specialize, clone, or eliminate it.

---

94. Assignment

Basic:

value = expression

Compound:

value += amount
value -= amount
value *= amount
value /= amount
value %= amount

value &= mask
value |= mask
value ^= mask

value <<= shift
value >>= shift

Assignment itself is a statement rather than a general expression.

This deliberately avoids C-style assignment-in-condition ambiguity.

---

95. Concurrency

Concurrency is available but not implicit in every "task".

Primary constructs are:

parallel
parallel each
spawn
await
detach

The compiler may also discover parallelizable work automatically where defined observable behavior permits it.

---

96. Parallel Block

Structured fork/join:

parallel
    update_audio()
    update_physics()
    update_animation()

The block completes only after its child operations complete.

The compiler may implement this using:

- OS threads;
- thread pools;
- work stealing;
- SIMD;
- sequential execution;
- target-specific scheduling.

If executing sequentially is cheaper and semantics allow it, "parallel" does not force wasteful thread creation.

---

97. Parallel Iteration

parallel each item in values
    transform(item)

This explicitly permits independent iterations to execute concurrently.

Conflicting unsynchronized access to shared non-atomic storage is undefined.

---

98. Spawn

worker := spawn calculate(data)

"spawn" produces an opaque execution handle.

Its implementation type is inferred.

---

99. Await

result := await worker

For value-producing work, "await" produces the value.

For "void" work, it establishes completion.

---

100. Detach

detach worker

A detached execution is not required to complete before the current operation proceeds or returns.

All referenced resources must remain valid for the detached work.

Lifetime violation is undefined.

---

101. Concurrency Data Races

Conflicting unsynchronized accesses to the same non-atomic storage from concurrent execution are undefined.

This gives the compiler freedom comparable to aggressive low-level native languages.

Atomic and synchronization facilities belong to the "atom" and "thread" standard facilities.

---

102. SIMD and Vectorization

Loops are optimizer opportunities by default.

each i in 0..<count
    output[i] = left[i] + right[i]

may lower into:

- scalar operations;
- SSE;
- AVX;
- AVX-512;
- ARM NEON;
- SVE;
- other target-native vector facilities.

Explicit SIMD source types are not required merely to permit automatic SIMD.

Low-level SIMD facilities may still exist for machine-specific tuning.

---

103. Machine Specialization

Instance binaries may target:

portable architecture target
architecture family
specific CPU generation
specific processor model
deployment machine

Machine-specialized compilation can exploit:

- instruction sets;
- cache characteristics;
- vector width;
- branch behavior;
- alignment;
- page size;
- target-specific instructions.

---

104. Runtime

Instance has no mandatory heavyweight runtime.

The minimum runtime contains only facilities actually needed by the compiled program.

Possible components include:

- process startup;
- IVS interface;
- platform error bridge;
- threading support where used;
- minimal standard-library runtime support.

Unused components should not be linked.

---

105. Standard Environment

The Instance standard environment is modular.

Representative namespaces include:

io
mem
vm
sys
thread
atom
math
simd
net
file
time
text
convert
process

Using one facility should not inherently pull unrelated subsystems into the executable.

---

106. Hidden Compilation Pipeline

The user conceptually sees:

.ins
 ↓
native binary

Internally an implementation may perform:

Instance Source
      ↓
EBNF Frontend
      ↓
Structural Parse
      ↓
Semantic Instruction Graph
      ↓
Type and Value Inference
      ↓
Definedness Analysis
      ↓
Alias Analysis
      ↓
Source/Derivation Analysis
      ↓
Storage Planning
      ↓
Speculation
      ↓
Graph Optimization
      ↓
Whole-Program Reduction
      ↓
C Backend Representation
      ↓
Native C Optimizer / Code Generator
      ↓
Object Code
      ↓
Native Link
      ↓
Executable Binary

This pipeline is not part of normal Instance programming.

Generated C:

- is an implementation artifact;
- is not the Instance programming model;
- is not normally persisted;
- is not user-facing source;
- must not be relied on textually by programs.

---

107. C-Based Backend

Instance lowers finalized surviving semantics into a C-oriented backend representation.

The C layer may supply:

- machine types;
- target ABI handling;
- arithmetic;
- memory operations;
- control flow;
- native library calls;
- operating-system interfaces;
- intrinsics;
- final native code generation.

Instance therefore uses C as a backend substrate, not as its source language.

---

108. C Is Not the Optimization Model

Instance is not a simple source-to-C translator.

Example:

task calculate(i32 value) -> i32
    temporary := value * 4
    give temporary / 2

need not become a C function.

It may reduce to:

value * 2

or disappear completely.

The hidden C backend receives surviving optimized semantics, not necessarily a recognizable translation of ".ins" source.

---

109. Optimization Contract

The Instance compiler is expected to aggressively attempt:

- constant folding;
- constant propagation;
- dead-code elimination;
- dead-store elimination;
- dead-load elimination;
- common-subexpression elimination;
- global value numbering;
- strength reduction;
- loop unrolling;
- loop fusion;
- loop fission;
- loop interchange;
- invariant hoisting;
- auto-vectorization;
- interprocedural analysis;
- whole-program optimization;
- devirtualization where relevant;
- escape analysis;
- scalar replacement;
- alias disambiguation;
- allocation removal;
- branch elimination;
- branch prediction;
- cold-path splitting;
- speculative specialization;
- callable cloning;
- task fusion;
- process fusion;
- node fusion;
- sequence dissolution;
- representation shrinking;
- stack promotion;
- register promotion;
- demand-storage planning;
- storage elimination;
- source/sink fusion.

These are central to the Instance programming model.

They are not decorative compiler extras.

---

110. Optimization as Semantic Consumption

Instance's most distinctive optimization idea is semantic consumption.

Source begins rich in meaning.

Compilation consumes that meaning to justify increasingly simpler machine behavior.

For example:

high-level task
      ↓
known task semantics
      ↓
known arguments
      ↓
known result
      ↓
constant
      ↓
known consumer
      ↓
consumer simplifies
      ↓
constant unnecessary
      ↓
nothing

The compiler has not lost the abstraction.

It has finished using it.

---

111. Instruction Dissolution

Instance considers deletion one of the most valuable optimizations.

The optimizer continually asks:

«Does this still need to exist?»

Candidates include:

- dead variables;
- dead stores;
- dead loads;
- dead branches;
- dead parameters;
- unused structures;
- redundant conversions;
- unnecessary bounds machinery;
- unnecessary allocations;
- duplicate expressions;
- repeated calculations;
- impossible failures;
- empty loops;
- obsolete temporaries;
- redundant nodes;
- materialized intermediate maps;
- unused storage;
- sequence boundaries.

---

112. Everything Semantic Participates

Instance follows the unusual rule:

«Nothing semantic is completely non-executable.»

A construct either:

1. contributes runtime behavior;
2. changes compilation;
3. establishes optimizer knowledge;
4. constrains storage;
5. determines representation;
6. influences code generation;
7. or disappears because its purpose has already been consumed.

A type declaration may emit no instruction, but it constrains representation.

An "assume" may emit no instruction, but it changes valid optimization.

A node may produce no callable, but it shapes graph reasoning.

A structure may disappear while determining layout.

Thus semantic source participates in execution formation even when it leaves no runtime residue.

Comments and insignificant formatting are excluded.

---

113. Compile-Time Folding

Instance treats compile-time evaluation as ordinary optimization.

Example:

task scale(i32 x) -> i32
    give x * 8

value := scale(10)

may become:

80

before backend lowering.

If "value" is unused, even "80" disappears.

---

114. Callable Elimination

task add(i32 a, i32 b) -> i32
    give a + b

process main() -> i32
    value := add(2, 3)
    give value

The executable does not owe the programmer:

- an "add" symbol;
- a call;
- a stack frame;
- parameters;
- a separate "value".

The result may reduce to:

give 5

and finally to the machine's native return convention for "5".

---

115. Process Fusion

process prepare()
    phase_a()
    phase_b()

process run()
    prepare()
    phase_c()

may conceptually become:

phase_a
phase_b
phase_c

and then be optimized further.

---

116. Task and Node Fusion

A chain such as:

decode
transform
normalize
output

may begin conceptually as:

decode
  ↓
buffer
  ↓
transform
  ↓
buffer
  ↓
normalize
  ↓
buffer

and reduce to:

load → decode → transform → normalize → store

with intermediate buffers dissolved.

---

117. Data Transformation Example

task transform(i32 value) -> i32
    give value * 2 + 4

process transform_all(i32[] data)
    each i in 0..<data.count
        data[i] = transform(data[i])

Possible conceptual machine form:

vector load
vector multiply/add
vector store

There may be:

- no "transform" call;
- no scalar loop;
- no temporary objects;
- no task representation.

---

118. Demand Storage Example

process simulate(size maximum)
    storage State[maximum] state

    each event in incoming
        index := event.index
        state[index] = update(state[index], event)

If "maximum" permits ten million logical states but only 80,000 are touched, IVS can reserve sufficient address space while physically backing only required regions.

The syntax stays simple.

The storage machinery remains mostly hidden.

---

119. Systems Programming Example

foreign c process write(int fd, byte* data, size count) -> int

process output(byte[] data) -> int
    result := write(1, data.ptr, data.count)

    when result < 0
        give -1

    give result

This remains close to native systems semantics without inheriting the full surface complexity of C.

---

120. Instructional Style Example

process launch(Config config) -> int
    validate(config)
    prepare(config)
    initialize_memory()
    initialize_workers()

    unless workers.ready
        fail EBUSY with -1

    start_workers()
    execute(config)
    stop_workers()

    give 0

This is quintessential Instance.

It reads like an operating procedure.

The compiler turns the procedure into machine execution.

---

121. What Instance Does Not Require

Instance does not inherently require:

- garbage collection;
- exception objects;
- stack unwinding;
- reflection metadata;
- dynamic dispatch;
- mandatory bounds checking;
- automatic runtime type information;
- reference counting;
- coroutine frameworks;
- virtual machines;
- JIT compilation;
- bytecode;
- hidden object headers;
- mandatory heap allocation.

Any feature requiring substantial runtime machinery must justify that machinery explicitly.

---

122. What Instance Is Designed For

Instance is particularly suited to:

- game engines;
- rendering;
- audio processing;
- video processing;
- simulation;
- scientific computation;
- databases;
- native servers;
- operating-system components;
- command-line software;
- compression;
- compilers;
- language runtimes;
- virtual machines;
- emulators;
- networking;
- packet processing;
- file processing;
- search systems;
- data transformation;
- numerical software;
- high-throughput infrastructure;
- realtime-adjacent applications;
- embedded systems;
- native libraries;
- compute kernels.

---

123. Where Instance Should Shine

Instance is strongest when:

1. substantial logic can be established before runtime;
2. throughput matters more than runtime dynamism;
3. data relationships can be inferred;
4. work can be fused;
5. loops dominate execution;
6. intermediate allocations can disappear;
7. memory regions are large or sparse;
8. C interoperability matters;
9. the programmer accepts definedness responsibility;
10. whole-program knowledge is available.

The more Instance understands, the more aggressively it can reduce.

---

124. Performance Philosophy

Instance does not guarantee that every ".ins" program automatically outruns carefully optimized C.

Compiler implementation, target hardware, program structure, and workload still matter.

Instance instead aims for a native performance ceiling while preserving higher-level semantic knowledge long enough for the optimizer to exploit it.

Its intended advantage is:

«Give the optimizer more knowledge while giving the runtime less responsibility.»

---

125. Instance Performance Equation

Conceptually:

Runtime Work
=
Requested Behavior
- Compile-Time Knowledge
- Proven Redundancy
- Foldable Computation
- Dissolved Abstraction
- Eliminated Storage
- Fused Operations
- Impossible Paths
- Consumed Semantics

Instance seeks to make the result as small as physically possible.

---

126. Language Personality

Instance source should feel:

- concise;
- instructional;
- deliberate;
- procedural;
- readable;
- deceptively high-level;
- mechanically powerful.

The programmer should feel that they are describing what the machine must accomplish rather than manually micromanaging every instruction required to accomplish it.

---

127. User-Typed Code Delineation

Instance does not add a dedicated construct such as:

user
user code

Ordinary ".ins" source is already the user-facing programming language.

Compiler-generated code is hidden.

Generated C is hidden.

Compiler-created temporary nodes are hidden.

Optimization-generated helper callables are hidden.

The meaningful visible distinction is:

Instance source
foreign code

and foreign code is already explicitly marked:

foreign c

Adding another marker for ordinary user code would create syntax without adding semantic information.

That conflicts with Instance's minimal-surface philosophy.

---

128. Canonical Surface Model

The finalized surface model is:

VALUES
    literals
    variables
    constants
    derived values
    sourced values
    tuples

MEMORY
    ordinary objects
    pointers
    references
    arrays
    slices/views
    IVS storage

WORK
    processes
    tasks
    nodes
    sequences

CONTROL
    when
    unless
    branch
    while
    until
    repeat
    each

TRANSFORMATION
    derive
    map
    sequencing operator

CONCURRENCY
    parallel
    parallel each
    spawn
    await
    detach

KNOWLEDGE
    assume
    noalias intrinsic

ERROR
    error
    check
    fail
    errno

STORAGE CONTROL
    storage
    commit
    decommit
    release

BOUNDARIES
    source
    foreign c

RESULT
    give

---

129. Authoritative Grammar Rule

Instance grammar is formally defined using Extended Backus–Naur Form.

EBNF is the authoritative syntactic baseline.

Parser behavior must never depend upon undocumented parser behavior.

The lexer may normalize blank and comment-only lines before layout parsing.

The parser receives synthesized layout tokens:

NL
INDENT
DEDENT
EOF

---

130. Lexical EBNF

letter
    = "A"…"Z"
    | "a"…"z"
    ;

digit
    = "0"…"9"
    ;

binary_digit
    = "0"
    | "1"
    ;

octal_digit
    = "0"…"7"
    ;

hex_digit
    = digit
    | "A"…"F"
    | "a"…"f"
    ;

identifier
    = ( letter | "_" ),
      { letter | digit | "_" }
    ;

decimal_integer
    = digit,
      { digit | "_" }
    ;

binary_integer
    = "0b",
      binary_digit,
      { binary_digit | "_" }
    ;

octal_integer
    = "0o",
      octal_digit,
      { octal_digit | "_" }
    ;

hex_integer
    = "0x",
      hex_digit,
      { hex_digit | "_" }
    ;

integer_literal
    = binary_integer
    | octal_integer
    | hex_integer
    | decimal_integer
    ;

exponent
    = ( "e" | "E" ),
      [ "+" | "-" ],
      decimal_integer
    ;

float_literal
    = decimal_integer,
      ".",
      decimal_integer,
      [ exponent ]

    | decimal_integer,
      exponent
    ;

escape
    = "\\n"
    | "\\r"
    | "\\t"
    | "\\0"
    | "\\\\"
    | "\\\""
    | "\\'"
    ;

string_char
    = ? any valid source character except quote,
        newline, or unescaped backslash ?
    ;

string_literal
    = "\"",
      { string_char | escape },
      "\""
    ;

char_literal
    = "'",
      (
          ? valid character except quote or backslash ?
        | escape
      ),
      "'"
    ;

bool_literal
    = "true"
    | "false"
    ;

null_literal
    = "null"
    ;

literal
    = integer_literal
    | float_literal
    | string_literal
    | char_literal
    | bool_literal
    | null_literal
    ;

---

131. Structural Tokens

The lexer synthesizes:

NL
INDENT
DEDENT
EOF

An "INDENT" occurs after a block-forming line when indentation increases by exactly four spaces.

One or more "DEDENT" tokens occur when indentation retreats.

Source:

when ready
    when fast
        run()
    finish()

Structural interpretation:

when ready NL
INDENT
    when fast NL
    INDENT
        run() NL
    DEDENT
    finish() NL
DEDENT

Blank and comment-only lines do not create indentation transitions.

---

132. Program Grammar

program
    = { NL | top_level_decl },
      EOF
    ;

top_level_decl
    = process_decl
    | task_decl
    | node_decl
    | sequence_decl
    | structure_decl
    | union_decl
    | enum_decl
    | foreign_decl
    | const_decl
    | global_decl
    ;

---

133. Callable Grammar

process_decl
    = "process",
      identifier,
      parameter_clause,
      [ return_clause ],
      block
    ;

task_decl
    = "task",
      identifier,
      parameter_clause,
      [ return_clause ],
      block
    ;

node_decl
    = "node",
      identifier,
      parameter_clause,
      [ return_clause ],
      block
    ;

sequence_decl
    = "sequence",
      identifier,
      parameter_clause,
      [ return_clause ],
      block
    ;

return_clause
    = "->",
      type
    ;

parameter_clause
    = "(",
      [ parameter_list ],
      ")"
    ;

parameter_list
    = parameter,
      { ",", parameter },
      [ ",", variadic_parameter ]

    | variadic_parameter
    ;

parameter
    = type,
      identifier
    ;

variadic_parameter
    = "..."
    ;

---

134. Type Grammar

type
    = base_type,
      { type_suffix }
    ;

base_type
    = primitive_type
    | tuple_type
    | identifier
    ;

primitive_type
    = "i8"
    | "i16"
    | "i32"
    | "i64"
    | "i128"

    | "u8"
    | "u16"
    | "u32"
    | "u64"
    | "u128"

    | "f16"
    | "f32"
    | "f64"
    | "f128"

    | "bool"
    | "byte"
    | "char"
    | "size"
    | "addr"
    | "int"
    | "void"
    | "error"
    | "text"
    ;

tuple_type
    = "tuple",
      "<",
      type,
      ",",
      type,
      { ",", type },
      ">"
    ;

type_suffix
    = pointer_suffix
    | reference_suffix
    | fixed_array_suffix
    | slice_suffix
    ;

pointer_suffix
    = "*"
    ;

reference_suffix
    = "&"
    ;

fixed_array_suffix
    = "[",
      constant_expression,
      "]"
    ;

slice_suffix
    = "[",
      "]"
    ;

---

135. Aggregate Grammar

structure_decl
    = "structure",
      identifier,
      aggregate_block
    ;

union_decl
    = "union",
      identifier,
      aggregate_block
    ;

aggregate_block
    = NL,
      INDENT,
      aggregate_member,
      { NL, aggregate_member },
      NL,
      DEDENT
    ;

aggregate_member
    = type,
      identifier,
      [ "=", expression ]
    ;

enum_decl
    = "enum",
      identifier,
      enum_block
    ;

enum_block
    = NL,
      INDENT,
      enum_member,
      { NL, enum_member },
      NL,
      DEDENT
    ;

enum_member
    = identifier,
      [ "=", constant_expression ]
    ;

---

136. Foreign Grammar

foreign_decl
    = "foreign",
      "c",
      foreign_callable,
      NL
    ;

foreign_callable
    = (
          "process"
        | "task"
      ),
      identifier,
      parameter_clause,
      [ return_clause ]
    ;

Foreign declarations do not have Instance bodies.

---

137. Global and Constant Grammar

const_decl
    = "const",
      type,
      identifier,
      "=",
      constant_expression,
      NL
    ;

global_decl
    = "global",
      type,
      identifier,
      [ "=", expression ],
      NL
    ;

---

138. Block Grammar

block
    = NL,
      INDENT,
      statement,
      { NL, statement },
      NL,
      DEDENT
    ;

Layout normalization removes blank and comment-only lines from structural significance before this grammar is applied.

---

139. Statement Grammar

statement
    = declaration_stmt
    | assignment_stmt
    | derive_stmt
    | source_stmt
    | storage_stmt

    | expression_stmt

    | when_stmt
    | unless_stmt
    | branch_stmt

    | while_stmt
    | until_stmt
    | repeat_stmt
    | each_stmt

    | parallel_stmt
    | parallel_each_stmt
    | detach_stmt

    | assume_stmt
    | check_stmt
    | fail_stmt

    | commit_stmt
    | decommit_stmt
    | release_stmt

    | give_stmt
    | break_stmt
    | continue_stmt
    | stop_stmt
    | skip_stmt
    ;

A sequencing pipeline is an expression and therefore requires no separate "pipeline_stmt" production.

---

140. Declaration Grammar

declaration_stmt
    = inferred_decl
    | typed_decl
    ;

inferred_decl
    = identifier,
      ":=",
      expression
    ;

typed_decl
    = type,
      identifier,
      [ "=", expression ]
    ;

derive_stmt
    = "derive",
      [ type ],
      identifier,
      "=",
      expression
    ;

source_stmt
    = "source",
      [ type ],
      identifier,
      "from",
      expression
    ;

The parser resolves optional type prefixes in "derive" and "source" through the established type-name namespace.

---

141. IVS Grammar

storage_stmt
    = "storage",
      type,
      "[",
      expression,
      "]",
      identifier,
      [ "=", expression ]
    ;

commit_stmt
    = "commit",
      storage_target
    ;

decommit_stmt
    = "decommit",
      storage_target
    ;

release_stmt
    = "release",
      identifier
    ;

storage_target
    = identifier,
      [
          "[",
          range_expression,
          "]"
      ]
    ;

Unlike fixed array type extents, the IVS storage extent may be a runtime expression.

---

142. Assignment Grammar

assignment_stmt
    = assignable,
      assignment_operator,
      expression
    ;

assignment_operator
    = "="
    | "+="
    | "-="
    | "*="
    | "/="
    | "%="
    | "&="
    | "|="
    | "^="
    | "<<="
    | ">>="
    ;

assignable
    = identifier
    | postfix_assignable
    ;

postfix_assignable
    = primary_expression,
      { member_access | index_access }
    ;

---

143. Conditional Grammar

when_stmt
    = "when",
      expression,
      block,
      { else_when_clause },
      [ else_clause ]
    ;

else_when_clause
    = "else",
      "when",
      expression,
      block
    ;

else_clause
    = "else",
      block
    ;

unless_stmt
    = "unless",
      expression,
      block,
      [ else_clause ]
    ;

---

144. Branch Grammar

branch_stmt
    = "branch",
      expression,
      NL,
      INDENT,
      branch_arm,
      { NL, branch_arm },
      [ NL, otherwise_arm ],
      NL,
      DEDENT
    ;

branch_arm
    = "case",
      branch_selector_list,
      [ "when", expression ],
      block
    ;

branch_selector_list
    = branch_selector,
      { ",", branch_selector }
    ;

branch_selector
    = constant_expression
    | range_expression
    | qualified_name
    ;

otherwise_arm
    = "otherwise",
      block
    ;

---

145. Loop Grammar

while_stmt
    = "while",
      expression,
      block
    ;

until_stmt
    = "until",
      expression,
      block
    ;

repeat_stmt
    = "repeat",
      expression,
      block
    ;

each_stmt
    = "each",
      each_binding,
      "in",
      expression,
      block
    ;

each_binding
    = identifier

    | identifier,
      ",",
      identifier
    ;

---

146. Concurrency Grammar

parallel_stmt
    = "parallel",
      block
    ;

parallel_each_stmt
    = "parallel",
      "each",
      each_binding,
      "in",
      expression,
      block
    ;

spawn_expression
    = "spawn",
      invocation_expression
    ;

await_expression
    = "await",
      expression
    ;

detach_stmt
    = "detach",
      expression
    ;

---

147. Error and Assumption Grammar

assume_stmt
    = "assume",
      expression
    ;

check_stmt
    = "check",
      expression
    ;

fail_stmt
    = "fail",
      expression,
      [ "with", expression ]
    ;

give_stmt
    = "give",
      [ expression ]
    ;

break_stmt
    = "break"
    ;

continue_stmt
    = "continue"
    ;

stop_stmt
    = "stop",
      [ expression ]
    ;

skip_stmt
    = "skip"
    ;

---

148. Mapping Grammar

map_expression
    = "map",
      map_binding,
      "in",
      pipeline_expression,
      "=>",
      expression
    ;

map_binding
    = identifier

    | identifier,
      ",",
      identifier
    ;

---

149. Formatting Grammar

format_expression
    = "format",
      string_literal,
      [ "with", argument_list ]
    ;

---

150. Range Grammar

range_expression
    = logical_or_expression,
      [
          range_operator,
          logical_or_expression,
          [ "by", logical_or_expression ]
      ]
    ;

range_operator
    = ".."
    | "..<"
    ;

---

151. Sequencing Grammar

pipeline_expression
    = range_expression,
      {
          "->",
          pipeline_stage
      }
    ;

pipeline_stage
    = identifier,
      [ pipeline_arguments ]
    ;

pipeline_arguments
    = "(",
      [ argument_list ],
      ")"
    ;

The result of the preceding stage is logically inserted as the first input of the following stage.

Example:

value -> scale(4) -> clamp(0, 255)

is semantically comparable to:

clamp(scale(value, 4), 0, 255)

without requiring nested runtime calls.

---

152. Expression Precedence

From lowest binding to highest binding:

map / format / spawn / await
pipeline
range
or
and
comparison
bitwise OR
bitwise XOR
bitwise AND
shift
addition/subtraction
multiplication/division/modulo
cast
unary
postfix
primary

Assignment remains a statement rather than an expression.

---

153. Expression EBNF

expression
    = map_expression
    | format_expression
    | spawn_expression
    | await_expression
    | pipeline_expression
    ;

pipeline_expression
    = range_expression,
      {
          "->",
          pipeline_stage
      }
    ;

range_expression
    = logical_or_expression,
      [
          range_operator,
          logical_or_expression,
          [ "by", logical_or_expression ]
      ]
    ;

logical_or_expression
    = logical_and_expression,
      {
          "or",
          logical_and_expression
      }
    ;

logical_and_expression
    = comparison_expression,
      {
          "and",
          comparison_expression
      }
    ;

comparison_expression
    = bitwise_or_expression,
      [
          comparison_operator,
          bitwise_or_expression
      ]
    ;

comparison_operator
    = "=="
    | "!="
    | "<"
    | "<="
    | ">"
    | ">="
    ;

bitwise_or_expression
    = bitwise_xor_expression,
      {
          "|",
          bitwise_xor_expression
      }
    ;

bitwise_xor_expression
    = bitwise_and_expression,
      {
          "^",
          bitwise_and_expression
      }
    ;

bitwise_and_expression
    = shift_expression,
      {
          "&",
          shift_expression
      }
    ;

shift_expression
    = additive_expression,
      {
          shift_operator,
          additive_expression
      }
    ;

shift_operator
    = "<<"
    | ">>"
    ;

additive_expression
    = multiplicative_expression,
      {
          additive_operator,
          multiplicative_expression
      }
    ;

additive_operator
    = "+"
    | "-"
    ;

multiplicative_expression
    = cast_expression,
      {
          multiplicative_operator,
          cast_expression
      }
    ;

multiplicative_operator
    = "*"
    | "/"
    | "%"
    ;

cast_expression
    = unary_expression,
      {
          "as",
          type
      }
    ;

unary_expression
    = unary_operator,
      unary_expression

    | postfix_expression
    ;

unary_operator
    = "+"
    | "-"
    | "~"
    | "not"
    | "*"
    | "&"
    ;

---

154. Postfix EBNF

postfix_expression
    = primary_expression,
      { postfix_operator }
    ;

postfix_operator
    = call_suffix
    | index_access
    | member_access
    | pointer_member_access
    ;

call_suffix
    = "(",
      [ argument_list ],
      ")"
    ;

index_access
    = "[",
      expression,
      "]"
    ;

member_access
    = ".",
      identifier
    ;

pointer_member_access
    = "->",
      identifier
    ;

argument_list
    = expression,
      { ",", expression }
    ;

"pointer->member" is parsed as postfix pointer-member access.

Spaced use:

value -> normalize

is sequencing.

Canonical formatting therefore helps reinforce an already grammatically distinguishable semantic difference.

---

155. Invocation Grammar

invocation_expression
    = callable_reference,
      call_suffix
    ;

callable_reference
    = identifier
    | qualified_name
    | postfix_expression
    ;

The semantic analyzer must establish that the referenced expression is callable.

---

156. Primary Expressions

primary_expression
    = literal
    | identifier
    | qualified_name
    | tuple_literal
    | parenthesized_expression
    | sizeof_expression
    | alignof_expression
    ;

qualified_name
    = identifier,
      {
          ".",
          identifier
      }
    ;

tuple_literal
    = "(",
      expression,
      ",",
      expression,
      {
          ",",
          expression
      },
      ")"
    ;

parenthesized_expression
    = "(",
      expression,
      ")"
    ;

sizeof_expression
    = "sizeof",
      "(",
      (
          type
        | expression
      ),
      ")"
    ;

alignof_expression
    = "alignof",
      "(",
      type,
      ")"
    ;

---

157. Constant Expressions

constant_expression
    = expression
    ;

with the semantic restriction that the expression must be evaluable during compilation.

This keeps grammar small while allowing semantic analysis to determine constant legality.

---

158. Expression Statements

expression_stmt
    = expression
    ;

Example:

update()

If an expression's result is discarded and the operation has no defined observable side effect, the entire expression may disappear.

---

159. Derivation Model

Instance compilation interprets:

source raw from input()

derive parsed = parse(raw)

derive normalized = normalize(parsed)

normalized -> encode -> write

approximately as:

input
  │
  ▼
 raw
  │
  ▼
parse
  │
  ▼
parsed
  │
  ▼
normalize
  │
  ▼
normalized
  │
  ▼
encode
  │
  ▼
write

The compiler then attempts to reduce the graph toward:

source
  ↓
combined transformation
  ↓
sink

This is one of the defining ideas of Instance.

---

160. Semantic Meaning of High-Level Constructs

"source" means:

«This is an origin.»

"derive" means:

«This is a relationship, not necessarily an independently stored object.»

"node" means:

«This is a meaningful graph stage.»

"sequence" means:

«These stages form a related transformation path.»

"->" means:

«The preceding result feeds the following stage.»

"map" means:

«Apply this transformation logically across these elements.»

"branch" means:

«Dispatch one established subject among these alternatives.»

"assume" means:

«Treat this proposition as true for following defined execution.»

"storage" means:

«Provide logical addressable extent while allowing physical backing to follow demand.»

None of these constructs requires its surface abstraction to remain at runtime.

---

161. Complete Instance Example

foreign c process write(int fd, byte* data, size count) -> int

enum PacketKind
    control = 0
    data = 1
    command = 2
    telemetry = 3

structure Packet
    PacketKind kind
    byte[] payload
    bool valid

node decode(byte[] input) -> Packet
    assume input.count > 0

    Packet packet

    packet.kind = input[0] as PacketKind
    packet.payload = input
    packet.valid = true

    give packet

node normalize(Packet packet) -> Packet
    packet.payload = map value in packet.payload => value & 0x7F
    give packet

node verify(Packet packet) -> Packet
    assume packet.payload.count > 0

    unless packet.valid
        fail EINVAL with packet

    give packet

sequence prepare(byte[] input) -> Packet
    derive packet = input -> decode -> normalize -> verify
    give packet

task send(Packet packet) -> error
    result := write(
        1,
        packet.payload.ptr,
        packet.payload.count
    )

    when result < 0
        fail errno

    give 0

process main() -> int
    storage byte[64 * 1024 * 1024] receive_space

    commit receive_space[0..<65536]

    source incoming from receive(receive_space)

    unless incoming.valid
        give 1

    derive packet = prepare(incoming)

    branch packet.kind
        case PacketKind.control
            handle_control(packet)

        case PacketKind.data, PacketKind.telemetry
            check send(packet)

        case PacketKind.command
            handle_command(packet)

        otherwise
            fail EINVAL with 1

    release receive_space

    give 0

---

162. Final Architectural Identity

Instance can be summarized as:

High-Level Minimal Surface
          +
Instructional Procedural Structure
          +
Authoritative EBNF
          +
Tapered Indentation
          +
Processes / Tasks / Nodes / Sequences
          +
Source and Derivation Semantics
          +
Mapping and Branching
          +
Graph-Oriented Sequencing
          +
Native Systems Semantics
          +
Aggressive Type and Value Inference
          +
Context-Free Compiler Guessing
          +
Speculative Optimization
          +
Alias Freedom
          +
Defined Undefined Behavior
          +
Abstraction Dissolution
          +
Demand-Backed IVS
          +
C Error Semantics
          +
First-Class C Interoperability
          +
Hidden C Backend
          +
AOT Native Compilation
          =
INSTANCE

---

163. Canonical Compilation Model

PROGRAMMER
   │
   │  describes instructions,
   │  relationships,
   │  constraints,
   │  sources,
   │  transformations,
   │  storage,
   │  and required results
   ▼
INSTANCE SOURCE (.ins)
   │
   │  minimal tapered surface
   ▼
EBNF FRONTEND
   │
   ▼
SEMANTIC INSTRUCTION GRAPH
   │
   ├─ inference
   ├─ derivation analysis
   ├─ provenance analysis
   ├─ alias analysis
   ├─ definedness
   ├─ context-free guessing
   ├─ speculation
   ├─ storage planning
   ├─ specialization
   ├─ folding
   ├─ mapping fusion
   ├─ node fusion
   ├─ sequence collapse
   ├─ branch elimination
   ├─ representation reduction
   └─ dissolution
   │
   ▼
SURVIVING MACHINE WORK
   │
   ▼
HIDDEN C BACKEND
   │
   ▼
NATIVE CODE GENERATION
   │
   ▼
BINARY
   │
   ▼
DIRECT EXECUTION

The programmer never programs this pipeline.

The programmer programs Instance.

---

164. Final Language Rule

Instance source exists to give the compiler enough logical truth to destroy unnecessary machinery.

A source binding may disappear.

A derived value may disappear.

A tuple may decompose.

A structure may scalarize.

A node may disappear.

A sequence may collapse.

A branch may become constant.

A map may become SIMD.

A process may inline.

A task may fold.

A loop may unroll.

An array may never materialize.

A storage reservation may never physically commit.

An error path may be proven impossible.

A callable may never exist as a callable.

An entire transformation chain may become one machine operation.

And when the requested result is already established, the entire chain may reduce to nothing at runtime.

The formal Instance rule is:

«Anything logically established may be transformed. Anything provably unnecessary may be removed. Anything undefined imposes no required execution. Runtime contains only what survives.»

Instance ultimately treats a program as a temporary description of a result-producing machine.

The source is not sacred.

Processes are not sacred.

Tasks are not sacred.

Nodes are not sacred.

Sequences are not sacred.

Variables are not sacred.

Objects are not sacred.

Loops are not sacred.

Allocations are not sacred.

Representations are not sacred.

Abstractions are not sacred.

Only the program's defined observable requirements are sacred.

Everything else is negotiable.

Everything else may be transformed.

Everything else may disappear.

«INSTANCE — Define it. Infer it. Reduce it. Run what remains.»
