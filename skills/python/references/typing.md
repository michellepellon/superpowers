# Advanced Typing Patterns

Deep reference for Python's static typing system.

## Key PEPs

| PEP | Topic | Usage |
|-----|-------|-------|
| 484 | Type hints | Basic annotations |
| 544 | Protocols | Structural subtyping |
| 589 | TypedDict | Typed dictionaries |
| 695 | Type parameter syntax | Generic syntax (`class Foo[T]:`) |

## Protocol Patterns

```python
from typing import Protocol, runtime_checkable

@runtime_checkable
class Closeable(Protocol):
    def close(self) -> None: ...

class Connection:
    def close(self) -> None:
        print("Closed")

def cleanup(resource: Closeable) -> None:
    resource.close()

# Works - Connection satisfies Closeable protocol
cleanup(Connection())
```

## Generic Classes (PEP 695)

```python
# New syntax in Python 3.12+
class Stack[T]:
    def __init__(self) -> None:
        self._items: list[T] = []

    def push(self, item: T) -> None:
        self._items.append(item)

    def pop(self) -> T:
        return self._items.pop()

# Type-safe usage
stack = Stack[int]()
stack.push(1)
stack.push("string")  # Type error
```

## TypedDict for Structured Data

```python
from typing import TypedDict, NotRequired

class UserConfig(TypedDict):
    name: str
    email: str
    age: NotRequired[int]  # Optional key

def process_config(config: UserConfig) -> None:
    print(config["name"])  # Type-safe access
    print(config.get("age", 0))  # Safe default
```

## Variance Annotations

```python
from typing import TypeVar

# Covariant - can return subtype
T_co = TypeVar("T_co", covariant=True)

# Contravariant - can accept supertype
T_contra = TypeVar("T_contra", contravariant=True)

# Invariant (default) - exact type only
T = TypeVar("T")
```

## Dataclasses vs attrs

```python
from dataclasses import dataclass, field
import attrs

# Dataclass - stdlib, good default
@dataclass(frozen=True, slots=True)
class Point:
    x: float
    y: float

# attrs - more features, validation
@attrs.define(frozen=True)
class ValidatedPoint:
    x: float = attrs.field(validator=attrs.validators.gt(0))
    y: float = attrs.field(validator=attrs.validators.gt(0))
```

## Avoiding `Any`

```python
# Bad - loses type safety
def process(data: Any) -> Any:
    return data

# Good - use generics
def process[T](data: T) -> T:
    return data

# Good - use Union for known types
def parse(value: str | int | float) -> str:
    return str(value)

# Good - use object for truly unknown
def log(message: object) -> None:
    print(str(message))
```

## Signals of Mastery

- Codebases type-check cleanly without excessive `Any`
- Public APIs are stable, typed, and backward-compatible
- Types communicate intent, constraints, and invariants
- Refactors improve type clarity rather than degrade it
