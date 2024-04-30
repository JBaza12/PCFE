---
layout: post
title: Quiz
---
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Prostate Cancer Quiz</title>
</head>
<body>
    <h1>Prostate Cancer Quiz</h1>
    <b1>Please answer the following questions with a yes or a no.</b1>
    <br>
    <form id="quizForm">
        <div>
            <label for="q1">Q1. Do you have a history of prostate cancer?</label>
            <input type="text" id="q1" name="q1" required>
        </div>
        <div>
            <label for="q2">Q2. Have you noticed any changes in your urinary habits, such as increased frequency or difficulty urinating?</label>
            <input type="text" id="q2" name="q2" required>
        </div>
        <div>
            <label for="q3">Q3. Do you experience any pain or discomfort in your pelvic area, particularly during urination?</label>
            <input type="text" id="q3" name="q3" required>
        </div>
        <div>
            <label for="q4">Q4. Have you observed any blood in your urine?</label>
            <input type="text" id="q4" name="q4" required>
        </div>
        <div>
            <label for="q5">Q5. Have you experienced any persistent bone pain, especially in the back, hips, or thighs?</label>
            <input type="text" id="q5" name="q5" required>
        </div>
        <div>
            <label for="q6">Q6. Do you have difficulty starting or stopping the flow of urine?</label>
            <input type="text" id="q6" name="q6" required>
        </div>
        <div>
            <label for="q7">Q7. Have you noticed any swelling or discomfort in your testicles or scrotum?</label>
            <input type="text" id="q7" name="q7" required>
        </div>
        <div>
            <label for="q8">Q8. Have you experienced any symptoms of prostate enlargement, such as frequent urination, especially at night?</label>
            <input type="text" id="q8" name="q8" required>
        </div>
        <div>
            <label for="q9">Q9. Have you recently noticed a decrease in the force of your urine stream?</label>
            <input type="text" id="q9" name="q9" required>
        </div>
        <button type="submit" id="submitButton">Submit</button>
    </form>
    <!-- Add the button here -->
    <button onclick="showUserData()">Show My Data</button>
    <div id="result"></div>
    <script>
        document.getElementById('quizForm').addEventListener('submit', function(event) {
            event.preventDefault();
            // Get user answers from the form
            const formData = new FormData(this);
            const answers = Array.from(formData.values());
            // Send POST request to backend API
            fetch('http://127.0.0.1:8282/api/prostate-quiz/', {  // Update the URL to match your backend API endpoint
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ answers: answers })
            })
            .then(response => response.json())
            .then(data => {
                // Display the quiz result
                document.getElementById('result').textContent = data.result;
            })
            .catch(error => {
                console.error('Error:', error);
            });
        });
        function showUserData() {
            var confirmResult = confirm("Do you want to see your data?");
            // If the user confirms, redirect to the endpoint to fetch and display the data
            if (confirmResult) {
                fetch('/api/users')  // Assuming this endpoint returns user data
                    .then(response => response.json())
                    .then(data => {
                        // Display user data in the result div
                        document.getElementById('result').textContent = JSON.stringify(data);
                    })
                    .catch(error => {
                        console.error('Error:', error);
                    });
            }
        }
    </script>
</body>
</html>
