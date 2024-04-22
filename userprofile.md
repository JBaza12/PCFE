---
layout: default
title: Your Profile
course: compsci
permalink: /profile
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            min-height: 100vh;
            background-color: #f5f5f5;
        }
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 20px;
            background: #fff;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        .header-left {
            font-size: 0.9em;
            color: #666;
        }

        .header-right a {
            margin-left: 20px;
            text-decoration: none;
            color: #333;
            font-weight: bold;
        }

        .container {
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            width: 80%;
            max-width: 400px;
            text-align: center;
            margin-top: 20px; 
        }
        form {
            display: flex;
            flex-direction: column;
        }
        input[type="text"], input[type="date"] {
            padding: 10px;
            margin: 5px 0 20px;
            border-radius: 5px;
            border: 1px solid #ddd;
            width: calc(100% - 22px);
        }
        button {
            background-color: #ff0000;
            color: white;
            padding: 10px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background-color: #ff2f00;
        }
        h2, h3 {
            color: #333;
        }
    </style>
</head>
<body>
<div class="header">
    <div class="header-left">
        CompSci Blogs
        <br>
        August 2023 to June 2024
    </div>
    <div class="header-right">
        <a href="http://127.0.0.1:4100/PCRC/" class="homePage">Home</a>
    </div>
</div>
    <div class="container">
        <!-- <div id="profile-info">
            <p>Name: <span id="userName"></span></p>
            <p>Date of Birth: <span id="userDOB"></span></p>
            <p>Username: <span id="userUID"></span></p>
        </div> -->
        <h3>Edit Profile</h3>
        <form action = "javascript:updateUserProfile()">
            <label for="name">Name:</label>
            <input type="text" id="name" name="name"><br>
            <label for="uid">Username:</label>
            <input type="text" id="uid" name="username"><br>
            <label for="dob">Date of Birth:</label>
            <input type="text" id="dob" name="dob"><br>  
            <button>Update Profile</button>
        </form>
<form action="javascript:deleteUserProfile()">
<button id="deleteUser">Delete Account</button>
</form>
    </div>

<script>
    const name = localStorage.getItem("userUid"); 
    // const dob = localStorage.getItem("userDOB");  
    // document.getElementById("userName").value = name;
    // document.getElementById("userDOB").textContent = dob; 
    //  document.getElementById("userUID").textContent = uid 
    let intId = 48
    function dataGet(){
        var myHeaders = new Headers();
        myHeaders.append("Content-Type", "application/json");
        // var raw = JSON.stringify({
        //   "name": name,
        //   "uid": uid,
        //   "id": id,
        //   "dob": dob
        // });
        var requestOptions = {
            method: 'GET',
            headers: myHeaders,
            redirect: 'follow'
        };
        <!-- const userId = id -->
        fetch("http://127.0.0.1:8282/api/users/?uid=" + name, requestOptions)
         .then(response => {
            if (response.ok) {
                //console.log(enteredTopic + " has been searched");
                return response.json(); // Parse the JSON in the response body
            } else {
                console.error("Search failed");
                const errorMessageDiv = document.getElementById('errorMessage');
                errorMessageDiv.innerHTML = '<label style="color: red;">Search Failed</label>';
                throw new Error('Search failed');
            }
        })
        .then(data => {
            console.log(data); // You can see your fetched data here
            intId = data.id
            const dob = new Date(data.dob).toISOString().split('T')[0];
            const username = data.uid
            const firstName = data.name
            // const intId = data.id
            document.getElementById('name').value = firstName
            document.getElementById('dob').value = dob
            document.getElementById('uid').value = username

        })
        .catch(error => {
            // Handle any errors that occurred during the fetch() or in the promise chain
            console.error('Error:', error);
        });
    }
    function updateUserProfile() {

        const updatedName = document.getElementById("name").value;
        const updatedDob = document.getElementById("dob").value;
        const updatedUsername = document.getElementById("uid").value;

            fetch('http://127.0.0.1:8282/api/users/' + intId, {
        method: 'PUT',
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify({ 
            name: updatedName, 
            dob: updatedDob, 
            uid: updatedUsername 
        })
    })
    .then(response => {
        if (response.ok) {
            return response.json();  // If you have a JSON response, parse it
        } else {
            throw new Error('Update failed with status: ' + response.status);
        }
    })
    .then(updatedUser => {
        alert("User info successfully updated");
        console.log('User updated:', updatedUser);
        // Optionally, you can reload data or update the UI as needed
    })
    .catch(error => {
        console.error('Error:', error);
        alert("Error updating user: " + error.message);
    });
}
     function deleteUser() {
        // Make sure intId is available
        // if (!intId) {
        //     console.error("User ID is not available. Cannot delete user.");
        //     return;
        // }

        // Call the deleteUserProfile function to perform the deletion
        deleteUserProfile();
    }

    // Define the deleteUserProfile function to handle the actual deletion process
    function deleteUserProfile() {
        fetch('http://127.0.0.1:8282/api/users/' + intId, {
            method: 'DELETE',
            headers: {'Content-Type': 'application/json'}
        })
        .then(response => {
            if (response.ok) {
                console.log('User deleted:', intId);
                alert("User deleted successfully.");
                window.location.href = "/CollaboraDJAK/signup";
            } else {
                return response.json().then(error => {
                    throw new Error(error.message || "Failed to delete the user.");
                });
            }
        })
        .catch(error => {
            console.error('Error:', error);
            alert("Error deleting user" + error.message);
        });
    }

    
    
        window.onload = dataGet;
    </script>
</body>
</html>
