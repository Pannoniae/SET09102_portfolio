# Project work 2

## Issue description

This week, there were not too many open issues so I have chosen to work on cleaning the codebase up somewhat.
([issue #116](https://github.com/Software-Engineering-Red/MAUI-APP/issues/116), closed by [PR #119](https://github.com/Software-Engineering-Red/MAUI-APP/pull/119))

I have created a utility class to contain common logic related to `INotifyPropertyChanged` boilerplate.

```csharp
public class Utils
{
    /// <summary>
    /// Sets the property T on the given type and raises OnPropertyChanged.
    /// </summary>
    /// <typeparam name="T">The type of the property.</typeparam>
    /// <typeparam name="TType">The type of the enclosing type.</typeparam>
    /// <param name="storage">The backing field of the property.</param>
    /// <param name="value">The new value to set.</param>
    /// <param name="type">The enclosing type.</param>
    /// <param name="propertyName">The name of the property (passed to OnPropertyChanged)</param>
    /// <returns></returns>
    public static bool SetProperty<T, TType>(ref T storage, T value, TType type, [CallerMemberName] string propertyName = null)
    {
        if (EqualityComparer<T>.Default.Equals(storage, value))
        {
            return false;
        }
    
        storage = value;
        OnPropertyChanged<TType>(type, propertyName);
        return true;
    }
    
    /// <summary>
    /// Raises OnPropertyChanged on type T.
    /// </summary>
    protected static void OnPropertyChanged<T>(T type, [CallerMemberName] string propertyName = null)
    {
        var theEvent = type!.GetType().GetEvent("PropertyChanged");
        var backingField = type.GetType().GetField("PropertyChanged", BindingFlags.Instance | BindingFlags.NonPublic)!;
        var delegateInstance = backingField.GetValue(type) as PropertyChangedEventHandler;
        delegateInstance?.Invoke(type, new PropertyChangedEventArgs(propertyName));
    }
}
```

This uses reflection to correctly raise events and eliminate duplicated code found in every model, which improves the quality of the codebase.


## Code reviews
I have reviewed 4 people's code this week.

### My reviews of others' code

#### First
In the first review, I have raised several issues including improperly named classes and unexplained removal of comments.
Most of the feedback I raised was ignored (not responded to at all), and the comments were not restored but new ones were written.
The new comments were only line-by-line descriptions of what the code does, which is generally a code smell.

Also worth mentioning that this pull request directly targeted `master` instead of `develop`.

#### Second

This pull request was a general cleanup of the code for the most part. I did not find any problems with it, other than the fact that
it should have been split into different PRs. My feedback was "Looks good to me", there is nothing much to say here.

#### Third

In the third review, I was explicitly told what to write for the review. The code contained an obvious problem (violating the single responsibility principle in a method)
which was deliberately placed there in order to create an issue which could be fixed quickly, without having to deal with any actual issues. In terms of organisation theatre,
this probably works fine, although from a moral standpoint, I find this practice utterly reprehensible.

#### Fourth

This pull request was the most well-written. Code was properly commented and documented, clean code principles were followed
and the person really knew what they were doing.
This PR also targeted `master` instead of `develop` but I let the person know this and they have retargeted it so it correctly merges into `develop`.

I have raised two minor concerns - one was about the data type of a field and the other was about an unneeded cast.
They responded to both of my concerns properly and addressed them. This review was absolutely a pleasant experience.

### Reviews of my code
Two people have reviewed my code this week. This week, I have learned from my lesson and I asked multiple people to review my code in advance so
I will not be stuck without a review.

Sadly, the contents of the reviews themselves are not too good. The first review simply said "lgtm!" without any elaboration.
The second review was obviously ChatGPT-generated word salad. It is worth to mention that what ChatGPT generated was not
actually fit for purpose as a code review, it was only abstract things without referencing specific lines of code.


## Reflection

### The codebase
This week, we have reverted to the original codebase before the rewrite. Naturally, there is lots of duplicated code.
My work this week aimed to reduce that, but there is still lots of work to do in making the code actually maintainable and
reducing brittleness.
Sadly, there is a phenomenon where every second pull request targets `master` directly instead of `develop`.

This has lead to a state where `master` and `develop` are once again out of sync, with different PRs merged for each branch, quasi-independently.
Also, there are 9 PRs open as of writing this, with many not merged since last week or updated to the latest `develop` codebase.

This is not good for the project because resolving merge conflicts while making sure people's contributions are not lost
is a non-trivial endeavour, creating tons of needless work in rebasing and manually merging.

This week, I tried to raise awareness of these issues, and ensure team members target the `develop` branch.
My efforts have not been entirely successful but time will tell.

### Communication and teamwork

Teamwork was slightly better this week, more people have participated in team communication over Discord, which is an encouraging development.
Sadly, there seems to be a trend where people make intentional mistakes and tell others to "find the issue" in reviews.
This kind of defeats the point of the code reviews. Additionally, code is still merged in without properly addressing review comments
or adhering to the `Definition of Done`.

The overall problem with the processes is still that everything is done on a cargo-culting basis, without really understanding why things should be done that way.
Due to this, the processes only add friction and delay without contributing actual benefits in software quality, sadly.

### Conclusion

This week, I was much more proactive in identifying and proposing solutions to the team's issues.
I have reviewed four people's code and gave them meaningful feedback.
So far,  I don't see an appreciable change in anything but I try to be hopeful about the future.

I have also observed that there were people who *removed* comments and documentation, with one of them explicitly saying
they have done so due to not caring the slightest about the module. Making meaningful improvements in software quality is not feasible
if most people are indifferent towards the project, with some people even making it worse on purpose, applying social pressure to discourage improvements.

I am going to continue to clean the codebase up and advocate for having better quality standards in the team.

Overall, I have managed to better identify problems in other team members' code in reviews this week. Also,
I have increased code quality which is an improvement from last week, so I am satisfied with myself this week.