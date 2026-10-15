# Unity Test Framework: Quickstart

The Unity Test Framework package supports Edit Mode and Play Mode tests. Use tests for deterministic logic and to catch regressions after changes to health, inventory, saving, calculations, or state transitions.

## Add a test assembly
1. Install/confirm Test Framework through Package Manager.
2. Create a Tests folder and use the Unity Editor's test assembly workflow so test files compile in a test assembly.
3. Choose Edit Mode for ordinary C# logic that doesn't need a running scene; choose Play Mode for behavior requiring the Unity runtime.
4. Open Window → General → Test Runner and run the desired tests.

## Example logic test
Place this in a test assembly that references the runtime assembly containing a class named ScoreRules. This sample illustrates the NUnit shape; ScoreRules is an example dependency you must implement in your project before this test compiles.

    using NUnit.Framework;

    public sealed class ScoreRulesTests
    {
        [Test]
        public void PositivePointsIncreaseScore()
        {
            int score = 10;
            int points = 5;
            score += points;
            Assert.AreEqual(15, score);
        }
    }

For useful tests, put calculations/state transitions in methods that can be tested independently, rather than testing only private Unity callbacks. Add tests for boundaries: null inputs, zero/negative values, empty inventories, save file missing, and repeated damage.

## Test categories
- Edit Mode: fast tests for pure logic and editor-time behavior.
- Play Mode: tests for GameObjects, coroutines, physics, and scene interactions.
- Build validation/manual QA: platform behaviors that do not show up in Editor tests.

## Common mistakes
- Test script is compiled into the wrong assembly.
- Runtime assembly is not referenced from the test assembly.
- Test relies on unspecified scene/script execution order.
- A test only asserts the happy path and ignores edge cases.
