/* Global Styles */
body {
    font-family: Arial, sans-serif;
    background-color: #f8f9fb; /* Soft background */
    color: #333;
    margin: 0;
    padding: 0;
}

/* Container */
.container {
    display: flex;
    justify-content: center;
    padding: 40px 20px;
}

/* Main Body Container */
.main-body-container {
    background-color: #ffffff;
    border-radius: 12px;
    padding: 40px;
    width: 100%;
    max-width: 900px;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
    overflow: hidden;
    transition: all 0.3s ease-in-out;
}

.main-body-container:hover {
    box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15);
}

/* Tabs Navigation */
.tabs {
    display: flex;
    justify-content: center;
    margin-bottom: 30px;
}

.tabs button {
    padding: 12px 25px;
    background-color: #f1f1f1;
    border: 1px solid #ddd;
    border-radius: 50px;
    margin: 0 15px;
    font-size: 16px;
    font-weight: 500;
    color: #333;
    cursor: pointer;
    transition: all 0.3s ease;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.tabs button:hover {
    background-color: #af8cf6;
    color: white;
    transform: translateY(-2px);
}

.tabs button.active {
    background-color: #6C63FF;
    color: white;
    border: none;
    transform: scale(1.05);
}

/* Button Group Styles */
.button-group {
    display: flex;
    gap: 15px;
    align-items: center;
    margin-bottom: 30px;
    justify-content: center;
}

.button-group h2 {
    font-weight: 600;
    font-size: 18px;
    margin-right: 15px;
    color: #333;
}

/* Styled Buttons */
.button-group button {
    padding: 12px 20px;
    background-color: #f4f4f4;
    border: 1px solid #ddd;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.3s ease;
    font-size: 16px;
}

.button-group button.active {
    background-color: #af8cf6;
    color: white;
    border: none;
}

.button-group button:hover {
    background-color: #6C63FF;
    color: white;
    transform: translateY(-2px);
}

/* File Upload & Notification */
.upload-body-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-bottom: 20px;
}

.upload-body-container label {
    font-size: 18px;
    color: #555;
    margin-bottom: 12px;
}

.upload-body-container .upload-btn1 {
    padding: 12px 30px;
    background-color: #6C63FF;
    color: white;
    border-radius: 50px;
    font-size: 16px;
    cursor: pointer;
    transition: all 0.3s ease;
    margin-bottom: 20px;
}

.upload-body-container .upload-btn1:hover {
    background-color: #4e3bb7;
    transform: translateY(-2px);
}

.upload-body-container .file-upload {
    display: none;
}

.file-notification {
    color: #f44336;
    font-size: 16px;
    font-weight: 600;
    margin-top: 10px;
    text-align: center;
}

/* Audio Trimmer */
.media-selection-audio-trimmer {
    background-color: #f9f9f9;
    padding: 25px;
    border-radius: 8px;
    margin-top: 30px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.media-selection-audio-trimmer h3 {
    font-size: 18px;
    font-weight: 600;
    margin-bottom: 15px;
    color: #333;
}

.media-trimmer {
    background-color: #f2f2f2;
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 20px;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}

/* Dropdown Group */
.dropdown-group {
    display: flex;
    flex-direction: column;
    gap: 15px;
    margin-bottom: 20px;
}

.dropdown-group label {
    font-size: 16px;
    font-weight: 600;
    color: #333;
}

.dropdown-group select {
    padding: 12px;
    margin-top: 5px;
    border-radius: 8px;
    border: 1px solid #ddd;
    background-color: #fff;
    font-size: 16px;
    color: #333;
    transition: all 0.3s ease;
}

.dropdown-group select:focus {
    border-color: #6C63FF;
    box-shadow: 0 0 5px rgba(108, 99, 255, 0.5);
}

/* Script Upload Area */
.script-upload {
    margin-top: 20px;
}

.script-upload textarea {
    width: 100%;
    height: 100px;
    padding: 12px;
    border-radius: 8px;
    border: 1px solid #ddd;
    font-size: 14px;
    transition: all 0.3s ease;
}

.script-upload textarea:focus {
    border-color: #6C63FF;
    box-shadow: 0 0 5px rgba(108, 99, 255, 0.5);
}

.script-upload h4 {
    margin-bottom: 8px;
    font-weight: 600;
    font-size: 16px;
    color: #333;
}

/* Create Resource & Upload Media Buttons */
.create-resource-btn, .upload-media-btn {
    padding: 12px 25px;
    background-color: #28a745;
    color: white;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.3s ease;
    font-size: 16px;
    margin-top: 20px;
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

    .main-body-container {
        padding: 25px;
    }
}
