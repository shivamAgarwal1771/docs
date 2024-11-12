/* Tabs container and button styles */
.CallSummary-tabs {
    display: flex;
    gap: 12px;
    margin-bottom: 20px;
}

.CallSummary-tab-button {
    padding: 8px 18px;
    background-color: #f0f0f0;
    color: #444;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 16px;
    font-weight: 500;
    cursor: pointer;
    transition: background-color 0.3s, color 0.3s;
}

.CallSummary-tab-button:hover,
.CallSummary-tab-button:focus {
    background-color: #773D87;
    color: #fff;
}

/* General form container styling */
.contact-card, .CallSummary-cases, .CallSummary-interaction-history {
    background-color: #ffffff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    margin-bottom: 24px;
}

/* Section heading styles */
h2 {
    margin-bottom: 16px;
    font-size: 22px;
    color: #773D87;
}

/* Field container alignment */
.field-row, .case-section, .interaction-section {
    display: flex;
    flex-direction: column;
    gap: 12px;
    margin-bottom: 20px;
}

/* Input fields styling */
.CallSummary-input-field {
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 14px;
    width: 100%;
    box-sizing: border-box;
    transition: border-color 0.2s;
}

textarea {
    resize: vertical;
}

.CallSummary-input-field:focus {
    border-color: #773D87;
}

/* Add button styles */
.CallSummary-btn {
    padding: 8px 16px;
    background-color: #4CAF50;
    color: #ffffff;
    border: none;
    border-radius: 5px;
    font-size: 18px;
    cursor: pointer;
    font-weight: bold;
    transition: background-color 0.3s, transform 0.2s;
}

.CallSummary-btn:hover {
    background-color: #45a049;
}

.CallSummary-btn:active {
    transform: scale(0.98);
}

/* Remove button styling */
.CallSummary-remove-button {
    padding: 8px 16px;
    background-color: #e74c3c;
    color: #ffffff;
    border: none;
    border-radius: 5px;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
    transition: background-color 0.3s;
    align-self: center;
    width: auto;
}

.CallSummary-remove-button:hover {
    background-color: #c0392b;
}

/* Specific styles for case sections */
.CallSummary-attachments, .CallSummary-linked, .case-comments {
    margin-top: 20px;
}

h4 {
    font-size: 18px;
    color: #333;
}

/* Case and interaction section inner styling */
.CallSummary-attachments input,
.CallSummary-linked input,
.case-comments .comment-row input,
.case-comments .comment-row textarea {
    margin-bottom: 10px;
    padding: 10px;
    font-size: 14px;
}

/* Comments styling */
.case-comments .comment-row {
    display: flex;
    flex-direction: column;
    gap: 8px;
}

/* Overall form styling for alignment and spacing */
.CallSummary-form {
    display: flex;
    flex-direction: column;
    gap: 20px;
}
