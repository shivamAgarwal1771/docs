   if (this.customer360 && this.customer360.marketingAnalysis) {
      this.marketingAnalysis = this.customer360.marketingAnalysis;
      console.log("Marketing Analysis Information:", this.marketingAnalysis);
      // Now you can use this.marketingAnalysis as needed
    }
  } else if (error) {
    console.error("Error fetching record:", error);
  }
