/* General styles */
.call-summary-tabs {
    padding: 20px;
    background-color: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

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
}

.tab-button:hover {
    background-color: #0056b3;
}

/* Call Info styles */
.call-info, .call-notes, .resolution {
    margin-bottom: 20px;
}

.call-info-section, .call-note-section, .resolution-section {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin-bottom: 20px;
}

.input-field {
    padding: 12px;
    border: 1px solid #ccc;
    border-radius: 4px;
    width: 100%;
    box-sizing: border-box;
}

.btn {
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    background-color: #28a745;
    color: #fff;
    cursor: pointer;
    transition: background-color 0.3s;
}

.btn:hover {
    background-color: #218838;
}

.remove-button {
    background-color: #dc3545;
    padding: 10px 20px;
    border-radius: 4px;
}

.remove-button:hover {
    background-color: #c82333;
}

.option-section {
    display: flex;
    gap: 10px;
    align-items: center;
}

.option-section .input-field {
    flex: 1; /* Allow input to take up available space */
}
