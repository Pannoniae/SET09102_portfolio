# Project work 4

## Issue description

This week, I have implemented four issues
([#152](https://github.com/Software-Engineering-Red/MAUI-APP/issues/152),
[#162](https://github.com/Software-Engineering-Red/MAUI-APP/issues/162),
[#163](https://github.com/Software-Engineering-Red/MAUI-APP/issues/163),
[#166](https://github.com/Software-Engineering-Red/MAUI-APP/issues/166))
in PR [#192](https://github.com/Software-Engineering-Red/MAUI-APP/pull/192).
Because these four issues relate to operations, I had to create an Operation model and the associated page.

The first issue was about being able to view the status of operations, the second issue was about being able to abort operations,
the third issue was about being able to write and view a final report of an operation
and the fourth issue was about being able to view the details of the currently ongoing operations.
The new Operation page is capable of all four functions.

One novel thing this week is that we use an enum for the operation status.

```csharp
public enum OperationStatus
{
    NotStarted,
    InProgress,
    Completed,
    Aborted
}
```

*Figure 1: OperationStatus enum*

This is saved and loaded by converting the enum to a string.

```csharp
// in the save method
var operation = new Operation()
{
    ...
    Status = Enum.Parse<OperationStatus>(StatusEntry.SelectedItem as string),
    ....
};

// in the method where the operation is loaded
StatusEntry.SelectedItem = selectedOperation.Status.ToString();
```


## Code reviews

### Reviews of my code

I have got two reviews for my pull request. One was "Looks good" as usual, but the other one was more detailed this time.
That review is shown on Figure 2.

![review3.png](./images/review3.png)

*Figure 2: Code review for my pull request*

To remedy this, I have changed the constructor in this way:

```diff
public OperationPage()
{
    InitializeComponent();
    this.BindingContext = new Operation();
    this.operationService = new OperationService();
+}
-   Task.Run(async () => await LoadOperations());
+protected override async void OnAppearing()
+{
+   await LoadOperations();
    NameEntry.Text = "";
    }
```



### My reviews of others' code
This week, I have reviewed three pull requests.

#### First
[Pull Request #188](https://github.com/Software-Engineering-Red/MAUI-APP/pull/188).

#### Second
[Pull Request #190](https://github.com/Software-Engineering-Red/MAUI-APP/pull/190).

#### Third
[Pull Request #191](https://github.com/Software-Engineering-Red/MAUI-APP/pull/191).


## Reflection

*NOTE: This week's portfolio is written together with Week 10 due to the author having COVID.
It is split into two on a thematic basis, but the reflection chronologically applies to both Week 10 and 11.*

While being ill during week 10 and week 11, I was checking the team's Discord server but there has been radio silence.
Teamwork has effectively disappeared in those two weeks. Reviews seem to be handled by people working with their pair,
but there has been zero discussion on a team level. My pair has disappeared, so I am soliciting people individually to get my code reviewed,
which is a very suboptimal way of teamwork.
I have tried reaching out with various questions but team engagement is next to non-existent.

This week, I have undertaken the fairly large task of updating `master` to the `develop` changes.
This was not trivial due to the divergence between `master` and `develop`. This was a useful experience for me
as I have gained skills in merging branches on the command line and using Git from the terminal more effectively.
I have also read about various Git commands and familiarised myself a bit with vi.

I have also discovered a bug in the C# compiler where it confused two classes - `UndacApp.Resource` and `UndacApp.Models.Resource`,
which made an interface not compile due to the compiler thinking the generic parameter had the wrong type.
This probably happens because `UndacApp.Resource` is an auto-generated class of the application's resources and
.NET does not take namespaces into account properly.

In order to fix this, I had to rename the `Resource` model to `AResource` to make the project compile.

These things have improved my software engineering practice due to learning my tools better,
which makes me a more efficient software engineer.

I have also been proactive in importing issues from the SET09102 repository, because
I have observed that there were barely any open and unassigned issues to do.
Furthermore, I also reached out to people asking them to follow the workflow more closely, so
we can avoid merge issues in the future.


In terms of possible improvements, it would be beneficial to disable the ability on GitHub to merge a pull request
ithout addressing the reviewers' comments. It would also be great if the team would follow the agreed-on
workflow and engage with the other team members and the team project. The code itself also has no tests,
so a unit test suite would be a significant improvement.

Overall, I am going to continue being proactive with contributing improvements to the team project and exemplifying a standard of quality to the team.
Also, if the team's social landscape allows it, I will spearhead efforts to improve processes, workflow and team engagement.

# Project work 5

Week 12 is the fifth and last week in a series in which the goal is to improve your
personal software engineering practice. Your portfolio entry has the same general content
as last week's, including:

* A descriptive summary of the issue that you worked on.
* Snippets from your code with commentary showing how you have used good software design
  practice.
* A descriptive summary of the test code that you have written.
* A reflective summary of any changes that were requested during the code review along
  with your fixes.
* A descriptive summary of any issues you found with the code that you were asked to review.
* A general reflective section that identifies, for example,
  * New things you have realised this week
  * Common problems that can arise in a team development situation
  * How your practice compares to other people's
  * etc.

Be sure to include links to the original items in the team's GitHub repository.

In the reflective sections this week, you should highlight ways that you persona practice
has improved as before. It would also be good to reflect on any improvements that have
been made to the agreed team workflow and related procedures. Are things working
better than they were? What further improvements could be made in the future?

## Marking

The marks for this portfolio entry will be split as follows:

* Timeliness: 5 marks (one mark will be deducted for each day or partial day that the submission
  is late).
* Description: 1 mark is awarded for the descriptive content. You are guaranteed at least one
  question on this entry in the final interview assessment. Please do not skimp on the
  descriptive content.
* Reflection: 4 marks are awarded for the quality of reflection. Again, you should think of this
  as preparation for the interview assessment.
