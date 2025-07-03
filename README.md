# C# Naming Best Practice
| **Aspect**                   | **Best Practice**                                                                       | **Example**                       |
| ---------------------------- | --------------------------------------------------------------------------------------- | --------------------------------- |
| **Pascal Case**              | Use **PascalCase** for property names.                                                  | `FirstName`, `DateOfBirth`        |
| **Descriptive Names**        | Use meaningful, descriptive names that clearly indicate the property’s purpose.         | `EmployeeId` (not `Id`)           |
| **Avoid Abbreviations**      | Avoid short forms or abbreviations unless they are commonly understood.                 | `Address` (not `Addr`)            |
| **No Prefixes**              | Avoid prefixes like `m_`, `_`, or `Get`.                                                | `Name` (not `_Name`)              |
| **Avoid Hungarian Notation** | Do not include data type or scope in the property name.                                 | `Price` (not `dblPrice`)          |
| **Use Singular Names**       | Use singular form for properties that represent a single entity.                        | `Product` (not `Products`)        |
| **Plural for Collections**   | Use plural form for properties that represent a collection.                             | `Orders` (not `OrderList`)        |
| **Consistent Acronyms**      | For acronyms, capitalize the first letter only.                                         | `HtmlContent` (not `HTMLContent`) |
| **Follow Domain Language**   | Use terms from the domain language where applicable to align with business terminology. | `InvoiceNumber`, `TaxRate`        |
| **Booleans as Questions**    | Use a question-like naming convention for Boolean properties.                           | `IsActive`, `HasDiscount`         |

---

# C# Data Types
| **Type**   | **Description**                                  | **Default Value** | **Example**       | **Notes**                              |
| ---------- | ------------------------------------------------ | ----------------- | ----------------- | -------------------------------------- |
| `string`   | Sequence of Unicode characters                   | `null`            | `"Hello"`         | Immutable                              |
| `char`     | A single Unicode character                       | `'\0'`            | `'A'`             | Enclosed in single quotes              |
| `int`      | 32-bit signed integer                            | `0`               | `42`              | Range: -2,147,483,648 to 2,147,483,647 |
| `long`     | 64-bit signed integer                            | `0L`              | `123456789L`      | Use `L` suffix                         |
| `short`    | 16-bit signed integer                            | `0`               | `-32768`          | Range: -32,768 to 32,767               |
| `byte`     | 8-bit unsigned integer                           | `0`               | `255`             | Range: 0 to 255                        |
| `bool`     | Boolean value                                    | `false`           | `true`, `false`   | Used for flags or conditionals         |
| `double`   | 64-bit double-precision floating point number    | `0.0d`            | `3.14159`         | Use `d` suffix (optional)              |
| `float`    | 32-bit single-precision floating point number    | `0.0f`            | `3.14f`           | Use `f` suffix                         |
| `decimal`  | 128-bit precise decimal values for financial use | `0.0m`            | `99.99m`          | Use `m` suffix                         |
| `DateTime` | Date and time value                              | `01/01/0001`      | `DateTime.Now`    | Use `DateTime.Parse(...)` to convert   |
| `Guid`     | Globally unique identifier                       | `Guid.Empty`      | `Guid.NewGuid()`  | Used as unique keys                    |
| `object`   | Base type of all types                           | `null`            | Can hold any type | Use casting for type-specific behavior |

Special Notes:
Nullable types: Add ? to allow null values (e.g., int?, bool?, DateTime?).
Value types (like int, double) store data directly.
Reference types (like string, object) store a reference to the data.
