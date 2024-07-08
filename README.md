import { LightningElement, api, wire,track } from "lwc";
import INTERACTION_HISTORY from "@salesforce/resourceUrl/Interation_history";
import EYE_ICON from "@salesforce/resourceUrl/marketingEyes";
import marketDataJson from "@salesforce/resourceUrl/market_analysis";
// import fetchCustomerData from "@salesforce/apex/CustomerDataPageHandler.fetchCustomerData";
import {getRecord, getFieldValue} from 'lightning/uiRecordApi';
import CUSTOMER360_FIELD from '@salesforce/schema/Custom_Contact__c.Customer360__c'

// import Field from "@salesforce/schema/AccountHistory.Field";
 
export default class SimulationMarketingAnalysis extends LightningElement {
  @api recordId;
  @track customer360;
  icon = INTERACTION_HISTORY;
  eye = EYE_ICON;
  marketingAnalysis = [];
  analysis = [];
  @track marketAnalysisData ='';

  connectedCallback () {
    console.log("recordId", this.recordId);
    this.loadStaticData();
    console.log(this.customer360,"customer360")
  }


  loadStaticData() {
fetch(marketDataJson)
  .then(response => response.json())
  .then(data => {
    this.marketAnalysisData = data.description;
  })
  .catch(error => console.log(error));
  }

  @wire(getRecord,{recordId:"$recordId", fields:[CUSTOMER360_FIELD]})
  wiredRecord({ error, data }) {
    if (data) {
      console.log("customer360",this.recordId)
      this.customer360 = getFieldValue(data, CUSTOMER360_FIELD);
      console.log("customer360", this.customer360);
    } else if (error) {
      console.log(error);
    }
  }
  // @wire(fetchCustomerData, { recordID: "$recordId" })
  // wiredCustomerData({ error, data }) {
  //   console.log("recordID",this.recordId)
  //   if (data) {
  //     const parsedData = JSON.parse(data);
  //     console.log("parsedData :", parsedData.marketingAnalysis);
  //     this.analysis = parsedData.marketingAnalysis;
  //     this.marketingAnalysis = this.analysis;
  //     console.log("Marketing Analysis Information:", this.marketingAnalysis);
  //     // Convert rates object to array
  //     const ratesArray = Object.keys(this.marketingAnalysis.rates).map(
  //       (key) => ({
  //         key: key,
  //         value: this.marketingAnalysis.rates[key]
  //       })
  //     );
  //     this.rateData = ratesArray;
  //     console.log("rates Arry :", this.rateData);
  //   } else if (error) {
  //     console.error("Error fetching customer data:", error);
  //   }
  // }
}


{
    "interActionHistory": [
        {
            "resolution": "Resolved",
            "date": "14/01/24",
            "description": "complaint about high premium"
        },
        {
            "resolution": "Unresolved",
            "date": "14/01/24",
            "description": "Query about policy coverage, Complaint about claim processing"
        }
    ],
    "accountInformation": [
        {
            "labels": [
                {
                    "value": "savings",
                    "key": "Account Type"
                },
                {
                    "value": "Rs 10,000",
                    "key": "Balance"
                },
                {
                    "value": "Rs 200 withdrawal 14/2/24",
                    "key": "Last Transaction"
                }
            ],
            "headerValue": "12322435",
            "headerAttributer": "Account Number"
        },
        {
            "labels": [
                {
                    "value": "car loan",
                    "key": "Account Type"
                },
                {
                    "value": "Rs 12,00,000",
                    "key": "Liability"
                }
            ],
            "headerValue": "56778899",
            "headerAttributer": "Account Number"
        }
    ],
    "lifeEvents": [
        {
            "event": "recently married",
            "date": "12/1/24"
        },
        {
            "event": "new car",
            "date": "24/12/23"
        },
        {
            "event": "graduated",
            "date": "12/3/21"
        }
    ],
    "riskAssessment": {
        "summary": [
            {
                "description": "No late payments in the last 12 months"
            },
            {
                "description": "Low risk for loan default"
            }
        ],
        "riskCategory": "LOW",
        "creditScore": 750
    },
    "nextBestAction": [
        {
            "button": "Take Action",
            "description": "discuss the benefits of a higher-tier savings account"
        },
        {
            "button": "Take Action",
            "description": "offer a home loan considering his recent browsing session"
        },
        {
            "button": "Take Action",
            "description": "discuss about the FD schema"
        }
    ],
    "marketingAnalysis": {
        "button": "See All Customer Marketing Activities",
        "summary": [
            {
                "description": "responds well to email marketing (30% open rate)"
            },
            {
                "description": "has clicked on 3 FD schema promotional offers in app"
            }
        ],
        "rates": {
            "clickRate": "12",
            "openRate": "30"
        }
    },
    "customerInformation": {
        "email": "john@email.com",
        "contact": 495405045,
        "location": "Delhi",
        "gender": "male",
        "age": 35,
        "name": "John"
    }
}
