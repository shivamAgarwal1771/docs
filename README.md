import { useEffect } from 'react';

useEffect(() => {
    // Trigger "Add" for Life Events automatically when the component is loaded
    handleAdd(null, 'lifeEvents'); // This simulates clicking the + Add button

    // After adding the field, remove it after a short delay (for example, 2 seconds)
    setTimeout(() => {
        handleRemove(null, 'lifeEvents', 0); // Removes the first (newly added) life event
    }, 2000); // 2 seconds delay before removing
}, []);
