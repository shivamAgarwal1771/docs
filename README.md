    useEffect(() => {
        if (!metadata.customer360) {
            metadata['customer360'] = validateRequiredFields();
        }

        // Dispatch the initial data as soon as the component mounts
        dispatch(updateDemoData({ type: "metadata", data: metadata }));

    }, [metadata, dispatch]);
