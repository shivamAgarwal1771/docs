.nudge-layout {
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  font-family: Arial, sans-serif;
}

.nudge-types {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

.nudge-options {
  display: flex;
  gap: 20px;
}

.nudge-option {
  display: flex;
  align-items: center;
  cursor: pointer;
  padding: 8px 12px;
  background-color: #f0f0f0;
  border: 1px solid #ccc;
  border-radius: 5px;
  transition: background-color 0.3s ease;
}

.nudge-option:hover {
  background-color: #e0e0e0;
}

.main-content {
  display: flex;
  width: 100%;
  justify-content: space-between;
}

.left-section, .right-section {
  width: 48%; /* Adjust width to create space between left and right sections */
  padding: 10px;
}

h3 {
  margin: 15px 0;
}

.row {
  display: flex;
  align-items: center;
  margin-bottom: 15px; /* Space between rows */
}

.label {
  width: 150px; /* Fixed width for labels for alignment */
  font-weight: bold;
}

input[type="text"] {
  flex: 1; /* Allow input to grow */
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-left: 10px; /* Space between label and input */
}

.styled-button {
  padding: 8px 12px;
  background-color: #007bff; /* Bootstrap primary color */
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 10px; /* Space above buttons */
  transition: background-color 0.3s ease;
}

.styled-button:hover {
  background-color: #0056b3; /* Darker shade for hover effect */
}

.remove-button {
  background-color: #dc3545; /* Bootstrap danger color */
}

.remove-button:hover {
  background-color: #c82333; /* Darker shade for hover effect */
}

/* Responsive adjustments */
@media (max-width: 768px) {
  .main-content {
    flex-direction: column; /* Stack left and right sections on smaller screens */
  }

  .left-section, .right-section {
    width: 100%; /* Full width for stacked sections */
  }
}
