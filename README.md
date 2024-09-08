  useEffect(() => {
    const handleRouteChange = () => {
      // Reset all state variables when route changes
      setRender([]);  // Reset render state
      setHistory([]); // Reset history state
      setContact([]); // Reset contact state
      setCustomer([]); // Reset customer state
      setSummary([]);  // Reset summary state
      setTranscript([]); // Reset transcript state
      setCustomerInfo360({}); // Reset customerInfo360 state
      setDemoAgent(""); // Reset demoAgent state
      setDemoCustomer(""); // Reset demoCustomer state
      setTabs(""); // Reset tabs state
      setActivePage(true); // Reset activePage state
      setShowWiki(false); // Reset showWiki state
      setShowAutoAuditPopup(false); // Reset showAutoAuditPopUp state
      setSelectedQuestion({}); // Reset selectedQuestion state
      setHide(false); // Reset hide state
    };

    router.events.on("routeChangeStart", handleRouteChange); // Listen for route changes
    return () => {
      router.events.off("routeChangeStart", handleRouteChange); // Cleanup listener on unmount
    };
  }, [router]);
