/* Styles for the tabs */
.tabs {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
}

.tab-button {
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    background-color: #007bff;
    color: #fff;
    cursor: pointer;
    transition: background-color 0.3s;
    font-size: 16px;
}

.tab-button:hover {
    background-color: #0056b3;
}

.tab-button:focus {
    outline: none;
}

/* General styles for the form sections */
.contact-card, .cases, .interaction-history {
    background-color: #f9f9f9;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    margin-bottom: 20px;
}

h2 {
    margin-bottom: 20px;
    font-size: 24px;
}

/* Styles for the fields */
.field-row, .case-section, .interaction-section {
    display: flex;
    flex-direction: column;
    gap: 12px;
    margin-bottom: 20px;
}

.input-field {
    padding: 12px;
    border: 1px solid #ccc;
    border-radius: 4px;
    width: 100%;
    box-sizing: border-box;
    font-size: 14px;
}

textarea {
    resize: vertical;
}

/* Button styles */
.btn {
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    background-color: #28a745;
    color: #fff;
    cursor: pointer;
    transition: background-color 0.3s;
    font-size: 16px;
}

.btn:hover {
    background-color: #218838;
}

.btn:focus {
    outline: none;
}

.remove-button {
    background-color: #dc3545;
    padding: 10px 20px;
}

.remove-button:hover {
    background-color: #c82333;
}

/* Specific styles for case sections */
.attachments, .linked, .case-comments {
    margin-top: 16px;
}

h4 {
    margin-bottom: 10px;
    font-size: 18px;
}

/* Styles for individual input boxes */
.attachments input,
.linked input,
.case-comments .comment-row input,
.case-comments .comment-row textarea {
    margin-bottom: 10px;
}

.case-comments .comment-row {
    display: flex;
    flex-direction: column;
    gap: 8px;
}
