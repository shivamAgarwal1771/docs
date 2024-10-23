/* Main layout container */
.nudge-layout {
    padding: 20px;
    font-family: Arial, sans-serif;
    display: flex;
    flex-direction: column;
    gap: 20px;
}

/* Nudge Types Section at the top */
.nudge-types {
    display: flex;
    flex-direction: column;
    color: white;
    padding: 20px;
    background-color: #773D87;
    border: 2px solid #ccc;
    border-radius: 10px;
    margin-bottom: 20px;
}

.nudge-types div:first-child {
    font-size: 18px;
    font-weight: bold;
    margin-bottom: 10px;
}

.nudge-options {
    display: flex;
    justify-content: space-between;
}

.nudge-option {
    display: flex;
    align-items: center;
    gap: 5px;
    cursor: pointer;
    color: white;
    padding: 10px;
    border-radius: 5px;
    transition: background-color 0.3s ease;
}

.nudge-option:hover {
    background-color: #ddd;
}

.nudge-option span {
    font-weight: bold;
}

/* Main content layout with left and right sections */
.main-content {
    display: flex;
    gap: 30px;
}

/* Left Section: Details Section */
.left-section {
    flex: 1;
    padding: 20px;
    background-color: #f9f9f9;
    border: 1px solid #ccc;
    border-radius: 5px;
}

/* Ensure all field sections have equal height */
.field-section {
    display: flex;
    flex-direction: column;
    gap: 10px; /* Space between fields */
}

.left-section .row {
    margin-bottom: 10px;
}

.left-section .label {
    font-weight: bold;
    margin-bottom: 5px;
}

.left-section input {
    width: 100%;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

/* Right Section: Basic Info Section */
.right-section {
    flex: 1;
    padding: 20px;
    background-color: #f9f9f9;
    border: 1px solid #ccc;
    border-radius: 5px;
}

.right-section .row {
    margin-bottom: 20px;
}

.right-section input {
    width: 100%;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

/* Styled Button */
.styled-button {
    padding: 10px 15px;
    cursor: pointer;
    background-color: #773D87;
    color: white;
    border: none;
    border-radius: 4px;
    transition: background-color 0.3s;
}

/* Remove button style */
.remove-button {
    background-color: #e74c3c; /* Red color for remove button */
}

.remove-button:hover {
    background-color: #c0392b; /* Darker red on hover */
}

/* Equalize the height of the Call Context and other nudge types */
.field-section {
    min-height: 150px; /* Adjust height as needed */
}
