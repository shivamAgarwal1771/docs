.progress-bar-container {
    font-family: Arial, sans-serif;
    padding: 20px;
    margin-left: 14px;
}

.progress-bar {
    display: flex;
    align-items: center;
}

.progress-step {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    margin-right: 10px;
    position: relative;
}

.step-shape {
    width: 125px;
    height: 57px;
    margin-left: 38px;
    clip-path: polygon(0% 0%, 85% 0%, 100% 50%, 85% 100%, 0% 100%);
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
    margin-bottom: 5px;
    border: none;
    transition: background-color 0.3s ease;
}

.circle-number {
    width: 63px;
    height: 63px;
    background-color: white;
    color: black;
    border-radius: 50%;
    position: absolute;
    top: -3px;
    left: -19px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-weight: bold;
    border: 3px solid white;
}

.step-number {
    font-size: 14px;
}

.step-label {
    padding: 2px;
    color: white;
    font-size: 14px;
    max-width: 80px;
    margin-left: 42px;
}

.step-content {
    display: flex;
    flex-direction: row;
    gap: 10px;
    margin-top: 10px;
    padding: 20px;
    margin-left: 60px;
}

.navigation-buttons {
    position: fixed;
    margin-top: 20px;
    display: flex;
    align-items: end;
    justify-content: end;
    width: 100%;
    bottom: 1rem;
    right: 2rem;
}

.enabled {
    margin-right: 35px;
    padding: 8px 16px;
    font-size: 16px;
    background-color: #773D87;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    outline: none;
}
