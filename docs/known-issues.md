# Known issues

## ESRS questions while a scan is running (not fixed)

The Role line allows ESRS questions, but Output Discipline, Step 4, the review-complete sentence, and Rules still say "zero response text" between the start-row answer and the Step 6 table. So a question typed during a scan might go unanswered or hang, which already happened once. Questions before the scan starts or at the Yes / No / Review again prompt should work.

**Fix if it comes back:** add one "User questions" exception under Output Discipline: answer in prose, never explain the trigger matrix or list rows, then re-issue the same prompt or resume from the current row. Point the three silence lines at that exception. Also decide whether knowledge-base calls for user questions count against the one-per-batch limit.
