useEffect(() => {
    const timer = setTimeout(() => {
        // Only update if the risk category is still 'Low'
        if (metadata?.customer360?.riskAssessment?.riskCategory === 'Low') {
            const updatedMetadata = { ...metadata };  // Clone the metadata to avoid direct mutation
            updatedMetadata.customer360.riskAssessment.riskCategory = 'Medium';  // Change riskCategory to Medium
            handleInput('customer360', updatedMetadata);  // Update the parent component/store with the new metadata
        }
    }, 2000); // 2 seconds delay

    // Cleanup function to clear the timeout if the component unmounts or re-renders
    return () => clearTimeout(timer);
}, [metadata, handleInput]);
