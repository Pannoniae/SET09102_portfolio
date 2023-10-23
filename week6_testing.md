# Testing

## The tested code

The tested function generates a new random word and resets the display to the beginning of the game.
This is important to test because this is the initialisation of the game.

## Test 1

This test checks whether invalid challenges are rejected properly.
It fails if the method does not throw an exception.


```csharp
        [Theory]
        [InlineData("invalidtype")]
        [InlineData(null)]
        [InlineData("")]
        public void OnAttemptCreateNewBadChallenge(string gameType)
        {
            Assert.ThrowsAny<Exception>(() =>
            {
                var gamePage = new GamePage(gameType);
                gamePage.CreateNewChallenge();
            });
        }
```


## Test 2

This test checks whether a challenge can be successfully created.
It fails if the method throws an exception.

```csharp
        [Theory]
        [InlineData("Easy")]
        public void OnAttemptCreateNewChallenge(string gameType)
        {
            var gamePage = new GamePage(gameType);
            gamePage.CreateNewChallenge();
            Assert.True(true);
        }
```

## Evaluation

Please note that the tests have failed during the cross-testing but not due to any inherent problem with the tests themselves,
but due to environment configuration issues (no platform-specific TFM like `net7.0-windows` or `net7.0-android` defined when cross-testing).
