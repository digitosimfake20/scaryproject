# TODO: Implement Quiz Analysis System

## Backend Changes
- [ ] Modify analyzer.js to output exact JSON structure (score out of 10, simplified weakAreas, feedback only for wrong answers, recommendations with topic and nextQuestions)
- [ ] Add GET /api/questions/:quizId route in quiz.js to fetch questions for a quiz

## Frontend Changes
- [ ] Update QuizPage.jsx to fetch questions, render quiz interface, collect user answers with time tracking, submit to analysis, navigate to result
- [ ] Update ResultPage.jsx to receive and display analysis result (score, weakAreas, feedback, recommendations)

## Testing
- [ ] Test full flow: start quiz, answer questions, submit, view results
- [ ] Ensure score is out of 10, feedback only for wrongs, etc.
