Download Link: https://assignmentchef.com/product/solved-assignment-2-database
<br>
<h2>Schema</h2>

In this assignment, we will work with a database that could support the online tool MarkUs. MarkUs is “an open-source tool which recreates the ease and flexibility of grading assignments with pen on paper, within a web application. It also allows students and instructors to form groups, and collaborate on assignments.” (reference: http://markusproject.org/). By now, you have used MarkUs to hand in a few pieces of course work, so you have experienced some of its features.

Download these starter files from the course webpage:

<ul>

 <li>The database schema, ddl</li>

 <li>A very small sample data set (with just enough data to show you how to format your own), sql</li>

</ul>

You can simply download the files from a browser and save them into a directory of your choice. You can also use the command line to download the files. For example:

&gt; wget http://www.teach.cs.toronto.edu/~csc343h/winter/assignments/a2/schema.ddl

Your code for this assignment must work on <em>any </em>database instance (including ones with empty tables) that satisfies the schema, so make sure you read and understand it.

The schema definition uses several types of integrity constraints:

<ul>

 <li>Some attributes must be present (“NOT NULL”).</li>

 <li>Every table has a primary key (“PRIMARY KEY”). The attributes that are part of a primary key form a key in the sense we are used to (no repeats), and none of them may be NULL.</li>

 <li>Some tables define a set of attributes to be (“UNIQUE”). The attributes that are part of a set declared to be UNIQUE form a key in the sense we are used to (no repeats), but one or more of them can be NULL.</li>

 <li>Some tables use foreign key constraints (“REFERENCES”). By default, they will refer to the primary key of the other table.</li>

</ul>

<h3>Warmup: Getting to know the schema</h3>

To get familiar with the schema, ask yourself questions like these (but don’t hand in your answers):

<ul>

 <li>How would the database record that a student is working solo on an assignment?</li>

 <li>How would the database record that an assignment does not permit groups, that is, all students must work solo?</li>

 <li>Why doesn’t the Grader table have to record which assignment the grader is grading the group on?</li>

 <li>Can different graders mark the various members of a group in an assignment?</li>

 <li>Can different graders mark the various elements of the rubric for an assignment?</li>

 <li>In a rubric, what is the difference between “out of” and “weight”?</li>

 <li>How would one compute a group’s total grade for an assignment?</li>

 <li>How is the total grade for a group on an assignment recorded? Can it be released before the a grade was assigned?</li>

</ul>

<h2>Part 1: SQL Statements</h2>

In this section, you will write SQL statements to perform queries. For each query, you will insert the final results into a table; we will check the contents of that table to see if your query was correct.

We will provide files named q1.sql, q2.sql, …, q10.sql Each of these files defines the schema for the result table for that query, so that you will know exactly what attributes we are expecting, of what type, and in what order. Put each of your queries into the appropriate file, and make your final statement in each file be:

INSERT INTO qX (SELECT <em>… complete your SQL query here … </em>)

where X is the number of the query. This will populate the answer table with the correct tuples.

You are encouraged to use views to make your queries more readable. However, each file should be entirely self-contained, and not depend on any other files; each will be run separately on a fresh database instance, and so (for example) any views you create in q1.sql will not be accessible in q5.sql.

At the beginning of the DDL file, we set the search path to markus, so that the full name of every table etc. that we define is actually markus.<em>whatever</em>. You may set the search path to markus at the top of your query files too, so that you do not have to use the markus prefix throughout. But do not use the schema called public.

The output from your queries must exactly match the specifications in the question, including attribute names, attribute types, and attribute order. Row order does not matter.

Write SQL queries for each of the following. Note that every query can be written in SQL, and that the questions are not in order of difficulty.

Throughout, when we ask for an assignment grade, report it as a percentage, such as 82.5.

<ol>

 <li>We’d like to compare the grade distributions across assignments.</li>

</ol>

For each assignment, report the average grade, the number of grades between 80 and 100 percent inclusive, the number of grades between 60 and 79 percent inclusive, the number of grades between 50 and 59 percent inclusive, and the number of grades below 50 percent. Where there were no grades in a given range, report 0.

<table width="640">

 <tbody>

  <tr>

   <td width="141"><strong>Attribute</strong></td>

   <td width="499"> </td>

  </tr>

  <tr>

   <td width="141">assignment id</td>

   <td width="499">The assignment ID</td>

  </tr>

  <tr>

   <td width="141">average mark percent</td>

   <td width="499">The average grade for this assignment, as a percent</td>

  </tr>

  <tr>

   <td width="141">num 80 100</td>

   <td width="499">The number of grades between 80 and 100 percent inclusive</td>

  </tr>

  <tr>

   <td width="141">num 60 79</td>

   <td width="499">The number of grades between 60 and 79 percent inclusive</td>

  </tr>

  <tr>

   <td width="141">num 50 59</td>

   <td width="499">The number of grades between 50 and 59 percent inclusive</td>

  </tr>

  <tr>

   <td width="141">num 0 49</td>

   <td width="499">The number of grades below 50 percent</td>

  </tr>

  <tr>

   <td width="141"><strong>Everyone?</strong></td>

   <td width="499">Every assignment should appear, even if there are no grades associated with it yet.</td>

  </tr>

  <tr>

   <td width="141"><strong>Duplicates?</strong></td>

   <td width="499">No assignment should appear twice.</td>

  </tr>

 </tbody>

</table>

<ol start="2">

 <li><strong>Getting soft? </strong>We’d like to know if graders give higher and higher grades over time, perhaps due to fatigue. To investigate this, we only want to examine graders who’ve had lots of grading experience throughout the course.</li>

</ol>

For this question when we consider a grader’s average on an assignment, we will mean their average across individual students, not groups. For instance, the grade earned by a group of three students will contribute three times to the average.

Find graders who meet all of these criteria:

<ul>

 <li>They have graded (that is, they have been assigned to at least one group) on every assignment.</li>

 <li>They have completed grading (that is, there is a grade recorded in the Result table) for at least 10 groups on each assignment.</li>

 <li>The average grade they have given has gone up consistently from assignment to assignment over time (based on the assignment due date).</li>

</ul>

Report their name, the average across assignments of their average grade, and the increase between their average for the first assignment and their average for the last assignment. (For example, if the former is 72.5 percent and the latter is 82.9 percent, the increase is 10.4 percentage points.) You may assume that no two assignments have the same due date; this means that there is unambiguously one first and one last assignment.

<table width="651">

 <tbody>

  <tr>

   <td width="187"><strong>Attribute</strong></td>

   <td width="464"> </td>

  </tr>

  <tr>

   <td width="187">ta name</td>

   <td width="464">The name of a grader who meets the criteria, in this form:first name plus surname with a blank in between</td>

  </tr>

  <tr>

   <td width="187">average mark all assignments</td>

   <td width="464">The average across assignments of their average grade</td>

  </tr>

  <tr>

   <td width="187">mark change first last</td>

   <td width="464">The increase between their average for the first assignment and their average for the last assignment, as a number of percentage points.</td>

  </tr>

  <tr>

   <td width="187"><strong>Everyone?</strong></td>

   <td width="464">Include only graders who meet the criteria.</td>

  </tr>

  <tr>

   <td width="187"><strong>Duplicates?</strong></td>

   <td width="464">No grader can appear twice.</td>

  </tr>

 </tbody>

</table>

<ol start="3">

 <li><strong>Solo superior. </strong>We are interested in the performance of students who work alone vs in groups.</li>

</ol>

Find assignments where the average grade of those who worked alone is greater than the average grade earned by groups. For each, report the assignment ID and description, the number of students declared to be working alone and their average grade, the number of students (not groups) declared to be working in groups and the average grade across those groups (not students), and finally, the average number of students involved in each group, (include in this calculation those who worked solo).

If groups have been declared for an assignment but no grades have been recorded, you will be able to report counts but not average grades — the averages will have to be null.

<table width="647">

 <tbody>

  <tr>

   <td width="174"><strong>Attribute</strong></td>

   <td width="473"> </td>

  </tr>

  <tr>

   <td width="174">assignment id</td>

   <td width="473">ID of the assignment</td>

  </tr>

  <tr>

   <td width="174">description</td>

   <td width="473">Description of the assignment</td>

  </tr>

  <tr>

   <td width="174">num solo</td>

   <td width="473">The number of students declared to be working alone</td>

  </tr>

  <tr>

   <td width="174">average solo</td>

   <td width="473">The average grade among students who worked alone, or null if there were none who worked alone and have a grade recorded in Result.</td>

  </tr>

  <tr>

   <td width="174">num collaborators</td>

   <td width="473">The number of students (not groups) declared to be working in groups</td>

  </tr>

  <tr>

   <td width="174">average collaborators</td>

   <td width="473">The average grade across those groups (not students), or null if there were no groups that have a grade recorded in Result.</td>

  </tr>

  <tr>

   <td width="174">average students per group</td>

   <td width="473">The average number of students involved in each group,(include those who worked solo in this calculation)</td>

  </tr>

  <tr>

   <td width="174"><strong>Everyone?</strong></td>

   <td width="473">Every assignment should appear, even if no grades are available for it yet.</td>

  </tr>

  <tr>

   <td width="174"><strong>Duplicates?</strong></td>

   <td width="473">No assignment should appear twice.</td>

  </tr>

 </tbody>

</table>

<ol start="4">

 <li><strong>Grader report </strong>We want to make sure that graders are giving consistent grades.</li>

</ol>

For each assignment that has any graders declared, and each grader of that assignment, report the number of groups they have already completed grading (that is, there is a grade recorded in the Result table), the number they have been assigned but have not yet graded, and the minimum and maximum grade they have given.

<table width="512">

 <tbody>

  <tr>

   <td width="113"><strong>Attribute</strong></td>

   <td width="399"> </td>

  </tr>

  <tr>

   <td width="113">assignment id</td>

   <td width="399">The ID of an assignment that has one or more graders declared</td>

  </tr>

  <tr>

   <td width="113">username</td>

   <td width="399">A grader who is declared for that assignment</td>

  </tr>

  <tr>

   <td width="113">num marked</td>

   <td width="399">The number of groups they have already graded</td>

  </tr>

  <tr>

   <td width="113">num not marked</td>

   <td width="399">The the number they have been assigned but have not yet graded</td>

  </tr>

  <tr>

   <td width="113">min mark</td>

   <td width="399">The minimum grade they have given for that assignment, or null if they have given no grades</td>

  </tr>

  <tr>

   <td width="113">max mark</td>

   <td width="399">The maximum grade they have given for that assignment, or null if they have given no grades</td>

  </tr>

  <tr>

   <td width="113"><strong>Everyone?</strong></td>

   <td width="399">Every assignment that has graders declared should appear.</td>

  </tr>

  <tr>

   <td width="113"><strong>Duplicates?</strong></td>

   <td width="399">No assignment should appear twice.</td>

  </tr>

 </tbody>

</table>

<ol start="5">

 <li><strong>Uneven workloads </strong>We want to make sure that graders have had fairly even workloads.</li>

</ol>

Find assignments where the number of groups assigned to each grader has a range greater than 10. For instance, if grader 1 was assigned 45 groups, grader 2 was assigned 58, and grader 3 was assigned 47, the range was 13 and this assignment should be reported. For each grader of these assignments, report the assignment, the grader, and the number of groups they are assigned to grade.

<table width="550">

 <tbody>

  <tr>

   <td width="96"><strong>Attribute</strong></td>

   <td width="455"> </td>

  </tr>

  <tr>

   <td width="96">assignment id</td>

   <td width="455">The ID of an assignment with a range (as described above) greater than 10</td>

  </tr>

  <tr>

   <td width="96">username</td>

   <td width="455">A grader for that assignment</td>

  </tr>

  <tr>

   <td width="96">num assigned</td>

   <td width="455">The number of groups this grader has been assigned</td>

  </tr>

  <tr>

   <td width="96"><strong>Everyone?</strong></td>

   <td width="455">Every assignment with a range over 10 should appear.</td>

  </tr>

  <tr>

   <td width="96"><strong>Duplicates?</strong></td>

   <td width="455">No assignment should appear more than once.No assignment-grader pair should appear more than once.</td>

  </tr>

 </tbody>

</table>

<ol start="6">

 <li><strong>Steady work. </strong>We’d like groups to submit work early and often, replacing early submissions that are flawed or incomplete with improved ones as they work steadily on the assignment.</li>

</ol>

For each group on assignment A1 (the assignment whose description is ‘A1’), report the group ID, the name of the first file submitted by anyone in the group, when it was submitted, and the username of the group member who submitted it, the name of the last file submitted, when it was submitted, and the username of the group member who submitted it, and the time between submission of the first and last file.

It is possible that a group submitted only 1 file. In that case the first file and the last file submitted are the same. It is also possible that two files could be submitted at the same time. In that case, report a row for every first-last combination for the group.

<table width="667">

 <tbody>

  <tr>

   <td width="100"><strong>Attribute</strong></td>

   <td width="373"> </td>

   <td width="194"> </td>

  </tr>

  <tr>

   <td width="100">group id</td>

   <td width="373">The ID of an A1 group</td>

   <td width="194"> </td>

  </tr>

  <tr>

   <td width="100">first file</td>

   <td width="373">The name of the first file submitted by anyone in the group</td>

   <td rowspan="7" width="194">or null if no file was submitted</td>

  </tr>

  <tr>

   <td width="100">first time</td>

   <td width="373">The timestamp for its submission</td>

  </tr>

  <tr>

   <td width="100">first submitter</td>

   <td width="373">The username of the group member who submitted it</td>

  </tr>

  <tr>

   <td width="100">last file</td>

   <td width="373">The name of the last file submitted by anyone in the group</td>

  </tr>

  <tr>

   <td width="100">last time</td>

   <td width="373">The timestamp for its submission</td>

  </tr>

  <tr>

   <td width="100">last submitter</td>

   <td width="373">The username of the group member who submitted it</td>

  </tr>

  <tr>

   <td width="100">elapsed time</td>

   <td width="373">The time between the first and last submission for this group</td>

  </tr>

  <tr>

   <td width="100"><strong>Everyone?</strong></td>

   <td width="373">Every group defined for A1 should appear.</td>

   <td width="194"> </td>

  </tr>

  <tr>

   <td width="100"><strong>Duplicates?</strong></td>

   <td width="373">A group may occur more than once if there is a tie for its first and/or last submission.</td>

   <td width="194"> </td>

  </tr>

 </tbody>

</table>

<ol start="7">

 <li><strong>High coverage </strong>We are interested in identifying graders who have broad experience in this course.</li>

</ol>

Report the username of all graders who have been assigned at least one group (the group could be solo or larger) for every assignment and have been assigned to grade every student (whether in a solo or larger group) on at least one assignment.

<table width="403">

 <tbody>

  <tr>

   <td width="94"><strong>Attribute</strong></td>

   <td width="310"> </td>

  </tr>

  <tr>

   <td width="94">ta</td>

   <td width="310">The username of a grader who meets the criteria.</td>

  </tr>

  <tr>

   <td width="94"><strong>Everyone?</strong></td>

   <td width="310">Only graders who meet the criteria should appear.</td>

  </tr>

  <tr>

   <td width="94"><strong>Duplicates?</strong></td>

   <td width="310">No grader should appear more than once.</td>

  </tr>

 </tbody>

</table>

<ol start="8">

 <li><strong>Never solo by choice </strong>We are interested in the performance of students who chose to work in multi-student groups wherever possible.</li>

</ol>

For this question, assume that at least one assignment allows groups.

Find students who never worked solo on an assignment that allows groups, and who submitted at least one file for every assignment (indicating that they did contribute to the group). Report their username, their average grade on the assignments that allowed groups, and their average grade on the assignments that did not allow groups.

<table width="500">

 <tbody>

  <tr>

   <td width="97"><strong>Attribute</strong></td>

   <td width="403"> </td>

  </tr>

  <tr>

   <td width="97">username</td>

   <td width="403">The username of a student who meets the criteria</td>

  </tr>

  <tr>

   <td width="97">group average</td>

   <td width="403">Their average grade on the assignments that allowed groups.(Assume that at least one assignment allows groups.)</td>

  </tr>

  <tr>

   <td width="97">solo average</td>

   <td width="403">Their average grade on the assignments that did not allow groups, or null if there were none.</td>

  </tr>

  <tr>

   <td width="97"><strong>Everyone?</strong></td>

   <td width="403">Every student who meets the criteria should appear.</td>

  </tr>

  <tr>

   <td width="97"><strong>Duplicates?</strong></td>

   <td width="403">No student should appear twice.</td>

  </tr>

 </tbody>

</table>

<ol start="9">

 <li><strong>Inseparable </strong>Report pairs of students who each did group work whenever the assignment permitted it, and always worked together (possibly with other students in a larger group).</li>

</ol>

<table width="539">

 <tbody>

  <tr>

   <td width="94"><strong>Attribute</strong></td>

   <td width="446"> </td>

  </tr>

  <tr>

   <td width="94">student1</td>

   <td width="446">The username of the student in the pair that comes first alphabetically</td>

  </tr>

  <tr>

   <td width="94">student2</td>

   <td width="446">The username of the student in the pair that comes second alphabetically</td>

  </tr>

  <tr>

   <td width="94"><strong>Everyone?</strong></td>

   <td width="446">Only pairs that meet the criteria should appear.</td>

  </tr>

  <tr>

   <td width="94"><strong>Duplicates?</strong></td>

   <td width="446">No pair should appear twice.</td>

  </tr>

 </tbody>

</table>

<ol start="10">

 <li><strong>A1 report </strong>We’d like a full report on the A1 grades per group, including a categorization.</li>

</ol>

Compute the grade out of 100 for each group on assignment A1, the difference between their grade and the average A1 grade across groups (negative if they are below average; positive if they are above average), and either “above”, “at”, or “below” to indicate whether they are above, at or below this average.




<table width="625">

 <tbody>

  <tr>

   <td width="137"><strong>Attribute</strong></td>

   <td width="488"> </td>

  </tr>

  <tr>

   <td width="137">group id</td>

   <td width="488">The ID of a group for the assignment whose description is ‘A1’</td>

  </tr>

  <tr>

   <td width="137">mark</td>

   <td width="488">The group’s grade on A1, as a percentage, or null if they have no grade</td>

  </tr>

  <tr>

   <td width="137">compared to average</td>

   <td width="488">The difference between the group’s grade and the average grade or null if they have no grade</td>

  </tr>

  <tr>

   <td width="137">status</td>

   <td width="488">Either “above”, “at”, or “below” to indicate whether they are above, at or below this average, or null if they have no grade</td>

  </tr>

  <tr>

   <td width="137"><strong>Everyone?</strong></td>

   <td width="488">Every group declared for A1 should appear.</td>

  </tr>

  <tr>

   <td width="137"><strong>Duplicates?</strong></td>

   <td width="488">No group should appear more than once.</td>

  </tr>

 </tbody>

</table>







<h2>Part 2: Embedded SQL</h2>

Part 2 will be provided in the next few days.

<h3>Additional tips</h3>

You will need to look up details about built in types (such as timestamps) and how to work with them. You may find the coalesce feature helpful. Become familiar with the documentation for postgreSQL. (But don’t waste time searching without purpose for features that you hope will save you.)

In your JDBC code for Part 2, some of your SQL queries may be very long strings. You should write them on multiple lines for readability, and to keep your code within an 80-character line length. But you can’t split a Java string over multiple lines. You’ll need to break the string into pieces and use + to concatenate them together. Don’t forget to put a blank at the end of each piece so that when they are concatenated you will have valid SQL. Example:

String sqlText =

“select client_id ”


“from Request r join Billed b on r.request_id = b.request_id ” + “where amount &gt; 50”;

This makes the string read like a query at the postgreSQL shell, except with some extra quotes and concatenation characters.

Here are some common mistakes and the error messages they generate (you may remember these from the exercise we did in class):

<ul>

 <li>You forget the colon:</li>

</ul>

cdf&gt; java -cp /local/packages/jdbc-postgresql/postgresql-9.4.1212.jar Example Error: Could not find or load main class Example

<ul>

 <li>You ran it on a machine other than dbsrv1</li>

</ul>

cdf&gt; java -cp /local/packages/jdbc-postgresql/postgresql-9.4.1212.jar: Example SQL Exception.&lt;Message&gt;: Connection to localhost:5432 refused. Check that the hostname and port are correct and that the postmaster is accepting TCP/IP connections.

Aside: Your prompt on cdf is probably different from mine. I set mine to “cdf&gt;”.

<h2>Important: How we will assess the correctness of your code</h2>

We will be testing your code in the CS Teaching Labs environment using PostgreSQL. More specifically, we will test your queries and your JDBC code using an autotester integrated into MarkUs. You will be able to run a self-test on MarkUs, using the same autotesting infrastructure. It is your responsibility to make sure your code runs in this environment before the deadline! <strong>Code which works on your machine but not on our autotester will earn a grade of zero for correctness, and will not be eligible for a remark request</strong>.

The self-test will confirm that your code connects to ours as expected, for instance, that your attribute types and attribute order are as specified (for the SQL query part) and that your method names and parameters are consistent with ours (for the JDBC part). It will also check that your code produces the correct results, but only for one very simple test case. The self-test will not run your code through a thorough test suite. It’s part of your job to plan and implement a thorough set of tests for your own code. If you do, it will sail through the thorough autotesting that we will ultimately use when grading.