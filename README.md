useEffect(() => {
    // This will automatically trigger the click of the "Add" button after 2 seconds
    const timer = setTimeout(() => {
        const addButton = document.querySelector('.add-life-event-btn');
        if (addButton) {
            addButton.click(); // Simulate button click
        }
    }, 2000); // Wait for 2 seconds before clicking

    return () => clearTimeout(timer); // Cleanup on component unmount
}, []);
