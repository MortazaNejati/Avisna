# Official Project Progress Report
## Development and Completion of the Fifth Version of the Avisna Language

**Report Date:** September 13, 2026  
**Subject:** Report of activities performed, errors resolved, identified limitations, and work plan for the next day

---

## 1. Description of Work Performed

In this stage of developing the fifth version of the **Avisna/Lume** language, the main focus was on developing Native geometric capabilities in the language core and harmonizing different layers of the compiler.

Activities were carried out across several layers:

### 1-1. Development of Basic Geometric Types

Three new types were added to the language type system and supported in various parts of the compiler:

- `Point`
- `Vector`
- `Angle`

These types were registered in the language AST, and then the necessary support for their detection and resolution was added in Semantic and Formatter.

### 1-2. Mapping Geometric Types to LLVM

In CodeGen, the mapping of geometric types to LLVM structures was performed.

For `Point` and `Vector`, a Native structure containing two `f64` values for the `x` and `y` coordinates was created.

Conceptually:

```text
Point  = { f64 x, f64 y }
Vector = { f64 x, f64 y }
```

In the internal ABI layer of CodeGen, the Pointer related to these structures is passed as `i64`.

### 1-3. Implementation of Geometric Constructors

The following constructors were supported in CodeGen:

```text
point(x, y)
vector(x, y)
```

These functions create the corresponding Native structure and store the coordinates in it.

### 1-4. Support for Field Access

Access to geometric components was also implemented and tested:

```avs
p.x
p.y

v.x
v.y
```

Semantic detects the type of `x` and `y` for `Point` and `Vector` as `F64`, and CodeGen also performs access to the Native fields.

### 1-5. Development of Vector Operations

The main operations on `Vector` were implemented and tested:

- `Vector + Vector`
- `Vector - Vector`
- `Vector * Scalar`
- `Scalar * Vector`
- `Vector / Scalar`

In this section, the logic of Semantic and CodeGen was harmonized so that the result of the operation matches the expected geometric type.

### 1-6. Development of Point and Vector Operations

Combined operations between `Point` and `Vector` were also developed:

- `Point - Point → Vector`
- `Point - Vector → Point`
- `Point + Vector → Point`
- `Vector + Point → Point`

For the above operations, in addition to Semantic, the Native CodeGen path was also completed.

### 1-7. Fixing the Operation Type Detection System

In `crates/avisna_codegen/src/expr/operators.rs`, the logic for detecting the result type of geometric operations was completed.

For example, for:

```text
Vector + Point
```

the result should be:

```text
Point
```

This was taken into account in the expression type determination logic, and then the corresponding Native CodeGen path was also added.

### 1-8. Fixing the Regression Related to Bool and Match

During previous tests, an inconsistency was identified in comparing `Bool` values in `match`.

In one path, LLVM was comparing the `Bool` value with `i32`, while the correct Bool value in LLVM is `i1`.

This section was fixed so that the pattern comparisons:

```avs
true
false
```

are performed with the correct LLVM type.

---

# 2. Errors Resolved

By the end of this stage, the following errors have been identified and resolved.

### 2-1. Failure to Recognize Geometric Types

The issue of missing types:

```text
Point
Vector
Angle
```

in the language type system was resolved.

### 2-2. Formatter Error

The addition of new types had caused an exhaustive match error in the Formatter.

This was resolved by adding processing of geometric types to the Formatter.

### 2-3. Failure to Resolve Geometric Types in Semantic

Detection of the names:

```text
Point
Vector
Angle
```

was added and tested in the type resolution system.

### 2-4. LLVM Type Mapping Issues

Mapping of geometric types to the appropriate LLVM type and creation of Native structures related to `Point` and `Vector` were performed.

### 2-5. Storage Issue for Geometric Variables

The way `Point` and `Vector` are stored in local variables in CodeGen was completed.

### 2-6. Bool Issue in LLVM

The regression related to comparing `Bool` in `match` was fixed.

Test:

```avs
fn main() -> int {
    let x = true;
    return match x {
        true => 1,
        false => 0,
    };
}
```

Ran without errors after the fix.

### 2-7. Vector Operations

The following operations were successfully tested after the necessary fixes:

```text
Vector + Vector
Vector - Vector
Vector * Scalar
Scalar * Vector
Vector / Scalar
```

### 2-8. Point - Point Operation

The operation:

```text
Point - Point
```

with the result:

```text
Vector
```

was implemented and tested.

### 2-9. Point - Vector Operation

The operation:

```text
Point - Vector
```

with the result:

```text
Point
```

was supported in Semantic and CodeGen.

### 2-10. Point + Vector Operation

The operation:

```text
Point + Vector
```

with the result:

```text
Point
```

was completed.

### 2-11. Vector + Point Operation

The last case examined was:

```text
Vector + Point
```

which initially encountered the following Semantic error:

```text
error[E105]: arithmetic operation requires numeric operands
```

After completing the relevant paths, the error was resolved and the test was successfully performed.

---

# 3. Unresolved Errors

At the end of today's work, within the scope of the activities performed, **no active and reported error remains that would prevent the continuation of geometric tests**.

However, this does not mean the complete completion of geometric capabilities or the fifth version. Items that still need to be reviewed or developed are mentioned in the limitations and tomorrow's plan sections.

---

# 4. Observed Limitations

During development, it became clear that supporting new Native types is not sufficient by only adding a Type to the AST, and several layers must be modified in a coordinated manner.

These layers include:

```text
AST
  ↓
Formatter
  ↓
Semantic
  ↓
Type Resolution
  ↓
Storage
  ↓
LLVM Type Mapping
  ↓
Native Structs
  ↓
Expression CodeGen
```

It was also found that the current ABI of Expression CodeGen passes the value as `i64` for some complex types, and for `Point` and `Vector`, this value in practice represents a Pointer to the Native structure.

This issue should be carefully considered in the development of future capabilities, especially when examining Return Type and geometric Function Parameters.

### Another Important Limitation

Full support for Functions to directly return types:

```text
Point
Vector
```

is still an independent topic and was not the main goal at this stage.

As a result, the development of Expression-Level geometric operations has been done, but it should not be considered as the complete completion of the ABI of geometric functions.

### Limitation Related to Angle

The `Angle` type has been added to the type system and LLVM, but its complete mathematical and semantic operations have not yet been developed at this stage.

---

# 5. Current Project Status

At the end of this stage, the initial infrastructure of Native Geometry in the language has been formed, and the main operations required for `Point` and `Vector` have been implemented at the Expression level.

Overall status:

| Capability | Status |
|---|---|
| `Point` in AST | Done |
| `Vector` in AST | Done |
| `Angle` in AST | Done |
| Formatter | Done |
| Semantic Type Resolution | Done |
| LLVM Mapping | Done |
| Native Point Struct | Done |
| Native Vector Struct | Done |
| `point(x,y)` | Done |
| `vector(x,y)` | Done |
| `Point.x / Point.y` | Done |
| `Vector.x / Vector.y` | Done |
| `Vector + Vector` | Done |
| `Vector - Vector` | Done |
| `Vector * Scalar` | Done |
| `Scalar * Vector` | Done |
| `Vector / Scalar` | Done |
| `Point - Point` | Done |
| `Point - Vector` | Done |
| `Point + Vector` | Done |
| `Vector + Point` | Done |
| Bool/Match regression | Resolved |
| Complete `Angle` operations | Remaining |
| Complete Point/Vector ABI in functions | Remaining |

---

# 6. Description of Tomorrow's Work

The focus of the next stage will be on continuing the development of geometric capabilities and examining their integration with the language more thoroughly.

Suggested priorities:

### 6-1. Completion of Remaining Geometric Operations

The operations required for `Point` and `Vector` that have not yet been included in the development scope will be examined.

Including:

- Unary operations
- Comparisons if required by the language
- Combining Geometry with other numeric operations
- Examining Type Inference behavior in different cases

### 6-2. Examination and Development of `Angle`

After stabilizing `Point` and `Vector`, the capabilities of the type:

```text
Angle
```

will be examined and appropriate Semantic and CodeGen will be designed for it.

### 6-3. Examination of Geometric Function ABI

The manner of using:

```text
Point
Vector
Angle
```

as:

- Function Parameter
- Function Return Type
- Local value
- Expression return value

will be examined to determine which parts the current ABI covers and what modifications are needed.

### 6-4. Completion of Regression Tests

For each geometric operation, independent tests will be created or completed in the Backend path to prevent the return of current errors in future changes.

### 6-5. Continuation of Fifth Version Development

After stabilizing the Geometry section, work will continue on other items defined for the fifth version, including Semantic capabilities and reducing unnecessary Syntax in the language.

---

## Summary

At the end of this stage, the Native Geometry infrastructure in Avisna/Lume has been significantly developed.

The three types `Point`, `Vector`, and `Angle` have entered the type system, and the main paths of AST, Semantic, and LLVM have been created for them. Also, the main operations of `Point` and `Vector` have been implemented and tested at the Expression level.

The most important result of this stage has been the successful overcoming of issues related to the harmonization of the **Type System, Semantic Checker, and LLVM Code Generator** for geometric types.

Currently, the necessary foundation for continuing Geometry development and moving toward more advanced language capabilities has been provided.

---

---

# 官方项目进展报告
## Avisna/Lume 语言第五版的开发与完善

**报告日期：** 2026年9月13日  
**主题：** 已完成活动、已解决错误、已识别限制及明日工作计划的报告

---

## 1. 已完成工作的说明

在 **Avisna** 语言第五版开发的这一阶段，主要重点是在语言核心中开发 Native 几何功能，并协调编译器的各个层。

活动在多个层面进行：

### 1-1. 基本几何类型的开发

三种新类型被添加到语言类型系统中，并在编译器的各个部分得到支持：

- `Point`
- `Vector`
- `Angle`

这些类型被注册到语言 AST 中，然后在 Semantic 和 Formatter 中添加了检测和解析它们所需的支持。

### 1-2. 将几何类型映射到 LLVM

在 CodeGen 中，完成了几何类型到 LLVM 结构的映射。

对于 `Point` 和 `Vector`，创建了一个包含两个 `f64` 值用于 `x` 和 `y` 坐标的 Native 结构。

概念上：

```text
Point  = { f64 x, f64 y }
Vector = { f64 x, f64 y }
```

在 CodeGen 的内部 ABI 层中，与这些结构相关的 Pointer 以 `i64` 形式传递。

### 1-3. 几何构造函数的实现

以下构造函数在 CodeGen 中得到支持：

```text
point(x, y)
vector(x, y)
```

这些函数创建相应的 Native 结构并在其中存储坐标。

### 1-4. 字段访问的支持

对几何组件的访问也已实现并测试：

```avs
p.x
p.y

v.x
v.y
```

Semantic 将 `Point` 和 `Vector` 的 `x` 和 `y` 类型检测为 `F64`，CodeGen 也执行对 Native 字段的访问。

### 1-5. Vector 运算的开发

对 `Vector` 的主要运算已实现并测试：

- `Vector + Vector`
- `Vector - Vector`
- `Vector * Scalar`
- `Scalar * Vector`
- `Vector / Scalar`

在此部分中，Semantic 和 CodeGen 的逻辑得到协调，使运算结果符合预期的几何类型。

### 1-6. Point 和 Vector 运算的开发

`Point` 和 `Vector` 之间的组合运算也已开发：

- `Point - Point → Vector`
- `Point - Vector → Point`
- `Point + Vector → Point`
- `Vector + Point → Point`

对于上述运算，除了 Semantic 之外，Native CodeGen 路径也已完成。

### 1-7. 修复运算类型检测系统

在 `crates/avisna_codegen/src/expr/operators.rs` 中，完成了几何运算结果类型检测的逻辑。

例如，对于：

```text
Vector + Point
```

结果应为：

```text
Point
```

这在表达式类型确定逻辑中得到考虑，然后也添加了相应的 Native CodeGen 路径。

### 1-8. 修复与 Bool 和 Match 相关的 Regression

在之前的测试过程中，发现在 `match` 中比较 `Bool` 值时存在不一致。

在一条路径中，LLVM 将 `Bool` 值与 `i32` 进行比较，而 LLVM 中正确的 Bool 值是 `i1`。

此部分已修复，使模式比较：

```avs
true
false
```

使用正确的 LLVM 类型进行。

---

# 2. 已解决的错误

到此阶段结束时，以下错误已被识别并解决。

### 2-1. 无法识别几何类型

语言类型系统中缺少以下类型的问题：

```text
Point
Vector
Angle
```

已解决。

### 2-2. Formatter 错误

添加新类型导致 Formatter 中出现 exhaustive match 错误。

通过向 Formatter 添加几何类型的处理，此问题已解决。

### 2-3. Semantic 中无法解析几何类型

以下名称的检测：

```text
Point
Vector
Angle
```

已在类型解析系统中添加并测试。

### 2-4. LLVM 类型映射问题

完成了几何类型到适当 LLVM 类型的映射，以及与 `Point` 和 `Vector` 相关的 Native 结构的创建。

### 2-5. 几何变量的存储问题

完成了 `Point` 和 `Vector` 在 CodeGen 局部变量中的存储方式。

### 2-6. LLVM 中的 Bool 问题

修复了 `match` 中比较 `Bool` 相关的 regression。

测试：

```avs
fn main() -> int {
    let x = true;
    return match x {
        true => 1,
        false => 0,
    };
}
```

修复后无错误运行。

### 2-7. Vector 运算

以下运算在必要的修复后成功测试：

```text
Vector + Vector
Vector - Vector
Vector * Scalar
Scalar * Vector
Vector / Scalar
```

### 2-8. Point - Point 运算

运算：

```text
Point - Point
```

结果为：

```text
Vector
```

已实现并测试。

### 2-9. Point - Vector 运算

运算：

```text
Point - Vector
```

结果为：

```text
Point
```

在 Semantic 和 CodeGen 中得到支持。

### 2-10. Point + Vector 运算

运算：

```text
Point + Vector
```

结果为：

```text
Point
```

已完成。

### 2-11. Vector + Point 运算

最后检查的案例是：

```text
Vector + Point
```

最初遇到以下 Semantic 错误：

```text
error[E105]: arithmetic operation requires numeric operands
```

在完成相关路径后，错误已解决，测试成功执行。

---

# 3. 未解决的错误

在今天工作结束时，在所执行活动的范围内，**没有残留会阻止几何测试继续进行的活跃且已报告的错误**。

然而，这并不意味着几何功能或第五版的完全完成。仍需审查或开发的项目在限制和明日计划部分中提及。

---

# 4. 观察到的限制

在开发过程中发现，支持新的 Native 类型仅仅向 AST 添加 Type 是不够的，必须协调地修改多个层。

这些层包括：

```text
AST
  ↓
Formatter
  ↓
Semantic
  ↓
Type Resolution
  ↓
Storage
  ↓
LLVM Type Mapping
  ↓
Native Structs
  ↓
Expression CodeGen
```

还发现，Expression CodeGen 的当前 ABI 对于某些复杂类型以 `i64` 形式传递值，对于 `Point` 和 `Vector`，此值实际上代表指向 Native 结构的 Pointer。

在开发未来功能时，尤其是在检查 Return Type 和几何 Function Parameter 时，应仔细考虑此问题。

### 另一个重要限制

完全支持 Functions 直接返回以下类型：

```text
Point
Vector
```

仍然是一个独立主题，在此阶段不是主要目标。

因此，Expression-Level 几何运算的开发已完成，但不应将其视为几何函数 ABI 的完全完成。

### 与 Angle 相关的限制

`Angle` 类型已添加到类型系统和 LLVM，但其完整的数学和语义运算在此阶段尚未开发。

---

# 5. 当前项目状态

在此阶段结束时，语言中 Native Geometry 的初始基础设施已形成，`Point` 和 `Vector` 所需的主要运算已在 Expression 级别实现。

总体状态：

| 功能 | 状态 |
|---|---|
| AST 中的 `Point` | 已完成 |
| AST 中的 `Vector` | 已完成 |
| AST 中的 `Angle` | 已完成 |
| Formatter | 已完成 |
| Semantic Type Resolution | 已完成 |
| LLVM Mapping | 已完成 |
| Native Point Struct | 已完成 |
| Native Vector Struct | 已完成 |
| `point(x,y)` | 已完成 |
| `vector(x,y)` | 已完成 |
| `Point.x / Point.y` | 已完成 |
| `Vector.x / Vector.y` | 已完成 |
| `Vector + Vector` | 已完成 |
| `Vector - Vector` | 已完成 |
| `Vector * Scalar` | 已完成 |
| `Scalar * Vector` | 已完成 |
| `Vector / Scalar` | 已完成 |
| `Point - Point` | 已完成 |
| `Point - Vector` | 已完成 |
| `Point + Vector` | 已完成 |
| `Vector + Point` | 已完成 |
| Bool/Match regression | 已解决 |
| 完整的 `Angle` 运算 | 剩余 |
| 函数中完整的 Point/Vector ABI | 剩余 |

---

# 6. 明日工作说明

下一阶段的重点将是继续开发几何功能，并更全面地检查其与语言的集成。

建议的优先事项：

### 6-1. 完成剩余的几何运算

尚未纳入开发范围的 `Point` 和 `Vector` 所需运算将被检查。

包括：

- Unary 运算
- 语言需要时的比较
- 将 Geometry 与其他数值运算组合
- 检查不同情况下的 Type Inference 行为

### 6-2. 检查和开发 `Angle`

在稳定 `Point` 和 `Vector` 之后，将检查以下类型的功能：

```text
Angle
```

并为其设计适当的 Semantic 和 CodeGen。

### 6-3. 检查几何函数 ABI

将检查以下类型的使用方式：

```text
Point
Vector
Angle
```

作为：

- Function Parameter
- Function Return Type
- 局部值
- Expression 返回值

以确定当前 ABI 覆盖哪些部分以及需要哪些修改。

### 6-4. 完成 Regression 测试

对于每个几何运算，将在 Backend 路径中创建或完成独立测试，以防止当前错误在未来的更改中回归。

### 6-5. 继续第五版的开发

在稳定 Geometry 部分后，将继续进行为第五版定义的其他项目，包括 Semantic 功能和减少语言中不必要的 Syntax。

---

## 总结

在此阶段结束时，Avisna/Lume 中的 Native Geometry 基础设施已得到显著开发。

三种类型 `Point`、`Vector` 和 `Angle` 已进入类型系统，并且已为它们创建了 AST、Semantic 和 LLVM 的主要路径。此外，`Point` 和 `Vector` 的主要运算已在 Expression 级别实现和测试。

此阶段最重要的成果是成功克服了与几何类型的 **Type System、Semantic Checker 和 LLVM Code Generator** 协调相关的问题。

目前，为继续 Geometry 开发并迈向更高级语言功能所需的基础已经提供。
