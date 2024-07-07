import { LightningElement, track, wire } from "lwc";
import contactDataJson from "@salesforce/resourceUrl/bnym_contact_data";
import contactIcon from "@salesforce/resourceUrl/smart_agent_contact_icon";
import { subscribe, MessageContext } from "lightning/messageService";
import visibility from "@salesforce/messageChannel/otherCompVisibility__c";

export default class SimulationContact extends LightningElement {
  @track contactData = [];
  @track showComponent = true;

  @wire(MessageContext)
  messageContext;

  contactIconUrl = `${contactIcon}#contactIcon`;

  async loadStaticData() {
    try {
      const response = await fetch(contactDataJson);
      if (response.ok) {
        const responseJson = await response.json();
        Object.keys(responseJson).forEach((key) => {
          this.contactData.push({
            field: key,
            value: responseJson[key]
          });
        });
      } else {
        console.error("Error in fetching contact data ", response.status);
      }
    } catch (err) {
      console.error("Error in fetching contact json data ", err);
    }
  }

  subscribeToMessages() {
    this.subscription = subscribe(this.messageContext, visibility, (message) =>
      this.handleMessage(message)
    );
  }

  handleMessage(message) {
    this.showComponent = message.show;
  }

  connectedCallback() {
    this.loadStaticData();
    this.subscribeToMessages();
  }
}
