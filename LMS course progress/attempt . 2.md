I want to implement "Course Progress" feature. This will show the progress bar for number of completed lectures in a course. When a video/lecture is completed for more than 80%, the lecture will be marked as completed, and the progress bar for course progress will be updated. 


### course progress check w localStorage

- update lecture progress completion based on time duration (>= 80% as completed)
- update the course progress `completed_lectures/total_lectures`

### replacing localStorage w DB

- Implemented
	- Added progress persistence to `pages/course/[id].tsx`
	- Throttled updates on timeupdate (every ~5s or ≥5% change) sending percent and watchTime.
	- Early upsert on play.
	- Completion upsert at ≥80% and sets progress=100, status="completed".
	- Caches row id client-side to avoid repeated lookups.
	- Temporary role display via /api/auth/role to surface current role and allowed roles.
	- Created server endpoint pages/api/course/lecture-progress.ts:
	- Uses admin GraphQL client to perform read-then-insert/update on mix_class_lecture_progress.
	- Accepts courseId, lectureId, userId, progress, status, watchTime and returns the row id.
	- Added /api/auth/role to read JWT claims and print defaultRole, allowedRoles, and userId.
- Root cause of DB insert failures
	- Client role (user/guest) lacks insert/update permissions on mix_class_lecture_progress. Hasura hides mutation fields for unauthorized roles, causing Apollo errors like “field 'insert_mix_class_lecture_progress(one)' not found in type: 'mutation_root'”.
	- Reads worked (empty array) confirming table visibility, but inserts were blocked by permissions.
	- Workaround: moved writes to a server API route that uses the admin client, bypassing client role restrictions.


