.media-selection-container {
    font-family: Arial, sans-serif;
    padding: 20px;
    max-width: 600px;
    margin: auto;
    background-color: #ffffff;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.button-group {
    display: flex;
    gap: 10px;
    align-items: center;
    margin-bottom: 20px;
}

.button-group h2 {
    font-weight: 600;
    font-size: 16px;
    margin-right: 10px;
}

.button-group button {
    padding: 10px 15px;
    border: 1px solid #ddd;
    background-color: #f4f4f4;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s, transform 0.2s;
}

.button-group button.active {
    background-color: #af8cf6;
    color: white;
    border: none;
}

.button-group button:hover {
    background-color: #af8cf6;
    color: white;
    transform: translateY(-2px);
}

.content {
    border: 1px solid #ddd;
    padding: 20px;
    margin-bottom: 20px;
    background-color: #fafafa;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.content h3 {
    font-weight: 600;
    font-size: 16px;
    margin-bottom: 10px;
}

.dropdown-group {
    display: flex;
    flex-direction: column;
    gap: 15px;
    margin-bottom: 20px;
}

.dropdown-group label {
    display: flex;
    flex-direction: column;
    flex-basis: 100%;
}

.dropdown-group select {
    padding: 10px;
    margin-top: 5px;
    border-radius: 4px;
    border: 1px solid #ddd;
}

.audio-trimmer h3 {
    margin: 10px 0;
}

.audio-trimmer {
    background-color: #f8f8f8;
    padding: 15px;
    border-radius: 4px;
    box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
}

.media-trimmer {
    margin-bottom: 20px;
    padding: 10px;
    background-color: #f2f2f2;
    border-radius: 4px;
    box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
}

.script-upload {
    margin-top: 10px;
}

.script-upload textarea {
    width: 100%;
    height: 100px;
    padding: 10px;
    border-radius: 4px;
    border: 1px solid #ddd;
    font-size: 14px;
}

.script-upload h4 {
    margin-bottom: 8px;
    font-weight: 600;
    font-size: 16px;
}

.create-resource-btn, .upload-media-btn {
    padding: 10px 20px;
    background-color: #28a745;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s, transform 0.2s;
}

.create-resource-btn:hover, .upload-media-btn:hover {
    background-color: #218838;
    transform: translateY(-2px);
}

/* Responsive Styles */
@media (max-width: 768px) {
    .button-group {
        flex-direction: column;
    }

    .dropdown-group {
        flex-direction: column;
    }

    .dropdown-group label {
        width: 100%;
    }
}
