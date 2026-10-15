# Unity Test Framework: Quickstart

The Unity Test Framework supports Edit Mode and Play Mode tests. Use it to catch regressions in score rules, inventory, damage state, calculations, and save/load decisions.

## Setup
1. Install/confirm Test Framework in Package Manager.
2. Create a test assembly using the Editor's test assembly workflow.
3. Choose Edit Mode for pure C# logic and Play Mode for behavior requiring Unity runtime, GameObjects, physics, or coroutines.
4. Open Window → General → Test Runner and run the desired tests.

## Example: extract logic, then test it

A pure C# class is easy to test without a scene. In your runtime assembly, save this as ScoreCounterModel.cs:

    public sealed class ScoreCounterModel
    {
        public int Score { get; private set; }

        public void Add(int points)
        {
            if (points > 0) Score += points;
        }
    }

In the test assembly, save as ScoreCounterModelTests.cs. It must reference the runtime assembly containing ScoreCounterModel.

    using NUnit.Framework;

    public sealed class ScoreCounterModelTests
    {
        [Test]
        public void AddPositivePointsIncreasesScore()
        {
            var model = new ScoreCounterModel();
            model.Add(5);
            Assert.AreEqual(5, model.Score);
        }

        [Test]
        public void NonPositivePointsAreIgnored()
        {
            var model = new ScoreCounterModel();
            model.Add(5);
            model.Add(0);
            model.Add(-2);
            Assert.AreEqual(5, model.Score);
        }
    }

These examples test real logic only if the game uses the same ScoreCounterModel class. Avoid testing duplicate sample logic that production code does not call.

## Test categories
- Edit Mode: fast tests for pure logic and editor-time behavior.
- Play Mode: tests for GameObjects, coroutines, physics, and scene interactions.
- Build/manual QA: platform behavior and device integration.

Add boundary tests for nulls, empty collections, zero/negative values, missing save files, malformed save data, and repeated events.

## Common mistakes
- Test script compiles in the wrong assembly.
- Runtime assembly is not referenced by tests.
- Test relies on unspecified scene execution order.
- Only happy path is tested.
