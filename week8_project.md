# Project work 1

## Issue description

The issue I have worked on was [issue #74](https://github.com/Software-Engineering-Red/MAUI-APP/issues/74),
resolved by [pull request #94](https://github.com/Software-Engineering-Red/MAUI-APP/pull/94).

It specifies that resource lists should be able to be viewed with filtering support based on type and location.

My pull request added the following lines:

```csharp
            "CREATE TABLE IF NOT EXISTS resource_types (Id INTEGER PRIMARY KEY AUTOINCREMENT, Name TEXT NOT NULL)",
            "CREATE TABLE IF NOT EXISTS resources (Id INTEGER PRIMARY KEY AUTOINCREMENT, Name TEXT NOT NULL, Type TEXT, Location TEXT)",
```

## Code review

My code was reviewed by three people.
It was helpfully pointed out to me that `Location` should really be a foreign key to another table instead, with a `NOT NULL` constraint.
This is absolutely right and it would have been the correct thing to do.
Sadly, I could not implement this, due to issues which are going to be described below.

The other two people did not point any issues out.

My code review went much better. I pointed out a naming issue (a class was named `mediaDB` instead of `MediaDB`, not conforming to the C# Style Guidelines)
and an architectural issue. He created a `public async Task InitiateDataBase()` method which he called at the top of every method, like so:
```csharp
/// <summary>
/// Retrieves all media agencies from the database
/// </summary>
/// <returns>List of media agencies</returns>
public async Task<List<MediaAgencies>> GetAllMediaAgenciesFromDB()
{
    await InitiateDataBase();
    return await _connection.Table<MediaAgencies>().ToListAsync();
}
```

I have commented that the database would be better initialised in the constructor, like so:
```csharp
public MediaDB(string databasePath)
{
    _connection = new SQLiteAsyncConnection(databasePath, Constants.Flags);
    _ = InitiateDataBase().Result;
}
```
The `.Result` hack is needed because C# does not allow an async method to be called from the constructor.
This would block until `InitiateDataBase` completed.

## Reflection

### The codebase

Overall, this week in the team was highly dysfunctional.
The week started with agreeing to use the rewritten codebase, which was pushed on the `master` branch.
This created a situation where `master` and `develop` were (and still are) entirely different codebases,
instead of `develop` being a staging branch for `master`.

However, there are additional problems. The rewritten codebase is effectively a single-page generic database table browser,
which is next to impossible to modify to work in a generic way, as every table needs different handling and business logic.
This is a huge stepback from the previous architecture where every table and feature had a separate page
and separate code. The previous version had highly duplicate code, but currently, development is effectively halted
because no one can add code to the current version.

The team is currently debating on going back to the previous codebase but no final decision has been made yet.
This exemplifies the next issue very well.

### Communication and teamwork

Communication inside the team is not working very well. There is a recurrent phenomenon where everything is done strictly on Sunday,
which makes teamwork impossible as everyone is working individually to produce something, not as a team.
Pull requests are often self-merged and not reviewed properly, with the only feedback being "looks good to me".

I try to show a fair amount of initiative, but it is challenging due to general disinterest from the team. The workflow document is very far from reality, with none of the items being followed consistently. The sensible solution would be
aligning theory with practice by implementing the theoretical processes and making sure people follow them.

However, the team has chosen the opposite direction by formalising the "buddy" system with 2 people each as the core basis of teamwork in the workflow,
which means that communication within the team and teamwork as a whole officially takes a backseat now.
This might also lead to cliques forming in the future, which may cause further problems.

### Conclusion

What I've learned is that fundamentally, a team project needs a critical ratio of members who are interested in the project itself.
Without that, the project can write ambitious workflows or documents but in practice, none of those will be followed
if most do not understand the guidelines or the "why"'s. In theory, we are a Scrum team, but in practice,
exactly zero Scrum-related processes have occurred, which is just depressing.

"Cargo-culting" practice, when people do things because they see more senior members do them but without understanding why they do it that way,
generally causes problems.
The branches and code reviews are a good example - the team implements them because it is "best practice",
but there are zero checks to prevent broken or subpar code from being submitted.

I, as a single person, have very limited means to change this situation without committing vastly oversized amounts of effort into it,
so I can't detail a plan to rectify these issues. I would say that the biggest problem right now is the mindset - which is not a simple task to fix -
with the majority of people simply not caring about the module at all.