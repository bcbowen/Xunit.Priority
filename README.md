# Xunit.Priority

Provides an `ITestCaseOrderer` that allows you to control the order of execution of Xunit tests within a class.

Based closely on the code at https://github.com/xunit/samples.xunit/tree/master/TestOrderExamples/TestCaseOrdering

**Note** that the Xunit folks [have](https://github.com/xunit/xunit/issues/980#issuecomment-248213473) [stated](https://github.com/xunit/xunit/issues/1301#issuecomment-305323239) that they don't believe that well-written unit tests should be dependent on being run in a particular order. Nevertheless, there are some testing scenarios which are not strict unit testing and which may require test ordering.

## Usage

Add the following attribute to classes for which you want tests run in order:

```csharp
[TestCaseOrderer(PriorityOrderer.Name, PriorityOrderer.Assembly)]
```

Then decorate your test methods with the `Priority` attribute.

```csharp
[Fact, Priority(-10)]
public void FirstTestToRun() { }

[Fact, Priority(0)]
public void SecondTestToRun() { }

[Fact, Priority(10)]
public void ThirdTestToRunA() { }

[Fact, Priority(10)]
public void ThirdTestToRunB() { }

[Fact]
public void TestsWithNoPriorityRunLast() { }
```

### Notes

- Priorities are evaluated in numeric order (including 0 and negative numbers)
- Multiple tests with the same priority will be run in alphabetical order
- Tests with no explicit `Priority` attribute are assigned priority `int.MaxValue` and will be run last

