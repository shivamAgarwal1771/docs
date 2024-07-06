{
    "description": "Has clicked on 3 FD scheme promotional offers in app",
}

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
  connectedCallback() {
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

<template>
    <div class="Parent_container">
      <div class="header_parent_container">
        <div class="header_parent_subContainer">
          <div class="slds-col slds-size_2-of-12 image_container">
            <img src={icon} alt="Life Events" />
          </div>
          <div class="slds-col slds-size_10-of-12 header_Label_container">
            Marketing Analysis
          </div>
        </div>
      </div>
      <div class="information-container">
        <div class="progress-container">
          <template for:each={rateData} for:item="rate">
            <div class="open-rate-container" key={rate.Id}>
              <div class="open-rate-sub-container">
                <div class="content-container">
                  {rate.key}
                </div>
                <div class="content-container">
                  {rate.value}
                </div>
              </div>
              <div>
                <lightning-progress-bar value={rate.value} size="large" variant="circular"
                  style="background-color: #B89EDD;"></lightning-progress-bar>
              </div>
            </div>
          </template>
        </div>
        <div class="summary-container">
          <ul class="summary-sub-container">
            Summary
            <template for:each={marketingAnalysis.summary} for:item="summary">
              <div class="details-container" key={summary.Id}>
                <li>
                  <span>
                    {summary.description}
                  </span>
                </li>
              </div>
            </template>
          </ul>
        </div>
        <div class="activity-container">
          <div class="activity-sub-container">
            <div class="slds-col slds-size_3-of-12 image-container"><img src={eye} alt="Eye Icon" /></div>
            <div class="activity-label-container">
              {marketingAnalysis.button}
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
