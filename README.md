    const response = await fetch(marketDataJson);
    if (response.ok) {
      const responseJson = await response.json();
      this.marketAnalysis.push(responseJson.description)
    }

    import marketDataJson from "@salesforce/resourceUrl/market_analysis";

    import { LightningElement, api, wire } from "lwc";
import INTERACTION_HISTORY from "@salesforce/resourceUrl/Interation_history";
import EYE_ICON from "@salesforce/resourceUrl/marketingEyes";
import fetchCustomerData from "@salesforce/apex/CustomerDataPageHandler.fetchCustomerData";

export default class SimulationMarketingAnalysis extends LightningElement {
  @api recordId;
  icon = INTERACTION_HISTORY;
  eye = EYE_ICON;
  marketingAnalysis = [];
  analysis = [];
  marketAnalysis =[];

  connectedCallback () {
    console.log("Current Record ID:", this.recordId);
  }

  @wire(fetchCustomerData, { recordID: "$recordId" })
  wiredCustomerData({ error, data }) {
    if (data) {
      const parsedData = JSON.parse(data);

      console.log("parsedData :", parsedData.marketingAnalysis);
      this.analysis = parsedData.marketingAnalysis;
      this.marketingAnalysis = this.analysis;
      console.log("Marketing Analysis Information:", this.marketingAnalysis);
      // Convert rates object to array
      const ratesArray = Object.keys(this.marketingAnalysis.rates).map(
        (key) => ({
          key: key,
          value: this.marketingAnalysis.rates[key]
        })
      );
      this.rateData = ratesArray;
      console.log("rates Arry :", this.rateData);
    } else if (error) {
      console.error("Error fetching customer data:", error);
    }
  }
}
