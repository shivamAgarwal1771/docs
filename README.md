<template>
    <div>

        <div class="container1 upperContainer">
            <div class="container1_1">
                <div>
                    <img src={autoAudit} alt="Burger_Menu" class="imgAutoAudit" />
                </div>
                <div class="AutoAuditHeader">

                    {autoAuditHeaderLabel}
                </div>
            </div>
            <div class="container">
                <div class="PassPercentage">
                    {passPercentage}
                </div>
                <div>
                    <img src={menu} alt="Burger_Menu" onclick={onOpenAccountClick} class="imgBurgerMenu" />
                </div>
                <div>
                    <img src={accordion} alt="Accordion" class={imageClass} onclick={toggleParentRotation} />
                </div>
            </div>
        </div>
        <template if:false={isSubtab}>

            <div class="container1_2">
                <div class={secondContainerClass}>
                    <div>
                        <ul>
                            <template for:each={checklist} for:item="process" for:index="index">
                                <li key={process.Id} class="checkListContainer">
                                    <div class="container liContainer">
                                        <div class="processQuestionContainer">
                                            {process.question}
                                        </div>
                                        <div class="leftIconContainer">
                                            <img src={process.icon} alt="Minus" class="checkListProcessIcon" />
                                            <img src={accordion} alt="Accordion" onclick={toggleRotation}
                                                class={iconClass} data-index={index} aria-controls={process.Id}
                                                data-rotation={process.rotation} data-id={index} />
                                        </div>
                                    </div>
                                    <div class="processRotationContainer">
                                        <template if:true={process.rotation}>
                                            <template for:each={process.counterQuestions} for:item="counterQuestion">
                                                <div key={counterQuestion.Id} class="counterQuestionContainer">
                                                    {counterQuestion.questionText}
                                                    <div class="radioButtonContainer">
                                                        <template for:each={counterQuestion.options} for:item="option">
                                                            <div key={option.value} class="radioInputContainer">
                                                                <input type="radio" name={counterQuestion.Id}
                                                                    value={option.value} checked={option.checked}
                                                                    disabled={option.disabled} />
                                                                <label
                                                                    class="radioLabelContainer">{option.label}</label>
                                                            </div>
                                                        </template>
                                                    </div>
                                                </div>
                                            </template>
                                        </template>
                                    </div>
                                </li>
                            </template>
                        </ul>
                    </div>
                </div>
            </div>

        </template>
    </div>

    <!-- <template if:true={isExpand}> -->
    <!-- <template if:true={isSubtab}>
        <div class="class={secondContainerClass}">
            <div class="container1 upperContainer">
                <div class="container1_1">
                    <img src={autoAudit} alt="Burger_Menu" class="imgAutoAudit" />
                    <div class="AutoAuditHeader">
                        {autoAuditHeaderLabel}
                    </div>
                </div>
                <div class="container">
                    <div class="PassPercentage">
                        {passPercentage}
                    </div>
                    <img src={menu} alt="Burger_Menu" onclick={onOpenAccountClick} style="margin-left: 2vh; height: 25px; width: 25px;" />
                </div>
            </div>
            <div class="container2_234">
                <div class="subTabChildContainer">
                    <div class="container2_2 container2_2a">
                        <div>
                            <ul>
                                <template for:each={checklist} for:item="process" for:index="index">
                                    <li key={process.Id} class="subCheckListContainer">
                                        <div class="container liContainer" onclick={questionHandler} data-index={index}
                                            aria-controls={process.Id}>
                                            <div class="processQuestionContainer2">
                                                {process.question}
                                            </div>
                                            <div class="processIconContainer">
                                                <img src={process.icon} alt="Icon" class="subProcessIconContainer" />
                                            </div>
                                        </div>
                                    </li>
                                </template>
                            </ul>
                        </div>
                    </div>
                    <div class="container2_22 container2_2a">
                        <template if:true={selectedCounterQuestions}>
                            <template for:each={selectedCounterQuestions} for:item="counterQuestion">
                                <div key={counterQuestion.Id} class="subCounterQuestionContainer">
                                    {counterQuestion.questionText}
                                    <div class="subRadioButtonContainer">
                                        <template for:each={counterQuestion.options} for:item="option">
                                            <div key={option.value} class="subRadioButtonInput">
                                                <input type="radio" name={counterQuestion.Id} value={option.value}
                                                    checked={option.checked} disabled={option.disabled} />
                                                <label class="subRadioButton">{option.label}</label>
                                            </div>
                                        </template>
                                    </div>
                                </div>
                            </template>
                        </template>
                    </div>
                </div>
            </div>
        </div>
    </template> -->
    <!-- </template> -->
</template>

import { LightningElement, track, api, wire } from 'lwc';
import BURGER_MENU from '@salesforce/resourceUrl/AutoAuditBurgerMenu';
import ICON_AUTOAUDIT from '@salesforce/resourceUrl/autoAuditIcon';
import ACCORDION from '@salesforce/resourceUrl/Accordion';
import MINUS from '@salesforce/resourceUrl/audtoAuditMinusSignIcon';
import RIGHT_ICON from '@salesforce/resourceUrl/autoAuditRightMarkIcon';
import WARNING_ICON from '@salesforce/resourceUrl/autoauditWarningIcon';
import { CurrentPageReference } from 'lightning/navigation';
import {AUTO_AUDIT_LABELS} from "c/exlConstants";
import ROTATION_LABEL from '@salesforce/label/c.awc_rotataion_label';
import CONTAINER_VISIBLE_LABEL from '@salesforce/label/c.awc_second_container_visible_label'
import CONTAINER_HIDDEN_LABEL from '@salesforce/label/c.awc_second_container_hidden_Label'
import commonChannel from '@salesforce/messageChannel/commonChannel__c';
import { publish, MessageContext } from 'lightning/messageService';

const incomingData = [
    {
        "section": "Qualifying and verifying the customer", 
        "auditStatus": "PASS",
        "questions": [
            {
                "question": "Customer validated",
                "auditResponse": "Yes",
                "options": ["Yes", "No"]
            },
            {
                "question": "Verify Customer Info",
                "auditResponse": "No",
                "options": ["Yes", "No"]
            }
        ]
    },
    {
        "section": "Call Greeting",
        "auditStatus": "PASS",
        "questions": [
            {
                "question": "Was the Customer Greeted",
                "auditResponse": "Yes",
                "options": ["Yes", "No"]
            },
            {
                "question": "What was the sentiment of greeting?",
                "auditResponse": "Positive",
                "options": ["Negative", "Positive", "Neutral"]
            }
        ]
    },
    {
        "section": "Customer Feedback",
        "auditStatus": "FAIL",
        "questions": [
            {
                "question": "Was the customer feedback taken",
                "auditResponse": "",
                "options": ["Yes", "No"]
            },
            {
                "question": "Customer feedback:",
                "auditResponse": "",
                "options": ["Yes", "No"]
            }
        ]
		},
        {
            "section": "Call Closing",
            "auditStatus": "FAIL",
            "questions": [
                {
                    "question": "Was the customer asked for other issues?",
                    "auditResponse": "",
                    "options": ["Yes", "No"]
                },
                {
                    "question": "Was the customer greeted?",
                    "auditResponse": "",
                    "options": ["Yes", "No"]
                }
            ]
            },
            {
                "section": "Query Identification",
                "auditStatus": "WARNING",
                "questions": [
                    {
                        "question": "Was the customer intent identified?",
                        "auditResponse": "Yes",
                        "options": ["Yes", "No"]
                    },
                     {
                        "question": "Was the required workflow run",
                        "auditResponse": "Yes",
                        "options": ["Yes", "No"]
                    },
                    {
                        "question": "Was the customer greeted?",
                        "auditResponse": "Neutral",
                        "options": ["Positive", "Neutral","Negative"]
                    }
                ]
                }
];

export default class AutoAuditTestingComponent_v3 extends LightningElement {
    @track checklist;
    @track isShowModal = false;
    menu = BURGER_MENU;
    zeroLabel = AUTO_AUDIT_LABELS.ZERO_LABEL;
    passLabel = AUTO_AUDIT_LABELS.PASS_LABEL; 
    visibleBlockLabel = CONTAINER_VISIBLE_LABEL;
    hiddenBlockLabel = CONTAINER_HIDDEN_LABEL;
    percentageLabel = AUTO_AUDIT_LABELS.PERCENTAGE_LABEL;
    failLabel = AUTO_AUDIT_LABELS.FAIL_LABEL;
    oneLabel = AUTO_AUDIT_LABELS.ONE_LABEL;
    rotationLabel = ROTATION_LABEL;
    dataIdLabel =AUTO_AUDIT_LABELS.DATA_ID_LABEL;
    navItemType = AUTO_AUDIT_LABELS.NAVITEM_TYPE_LABEL;
    autoAuditApiLable = AUTO_AUDIT_LABELS.AUTO_AUDIT_API_LABEL
     autoAuditHeaderLabel = AUTO_AUDIT_LABELS.AUTO_AUDIT_HEADER_LABEL;
    @track passPercentage = this.zeroLabel;

    autoAudit = ICON_AUTOAUDIT;
    processQuestion = [];
    processQuestion1 = [];
    changeStyle = true;
    accordion = ACCORDION;
    minus = MINUS;
    rightIcon = RIGHT_ICON;
    warning = WARNING_ICON;
    autoAuditUi_v1 = true;
    isExpand = false;
    @track isSubtab = false;
    @track selectedCounterQuestions = [];
    @track rotations = {};
    isSecondContainerVisible = true;
    isImageRotated = false; 
    @wire(MessageContext) messageContext;

    connectedCallback() {
        this.checklist = this.transformData(incomingData);
        this.initializeProcessQuestion();
        const firstQuestion = this.processQuestion[0];
        if (firstQuestion && firstQuestion.counterQuestions) {
            this.selectedCounterQuestions = firstQuestion.counterQuestions;
        }
    }

    publishToCommonChannel(action) {
        const messagePayload = {
            source: "AUTO_AUDIT",
            message: {
                action: action
            },
        };
        publish(this.messageContext, commonChannel, messagePayload);
    }


    toggleParentRotation() {
        // Toggle the visibility state
        this.isSecondContainerVisible = !this.isSecondContainerVisible;
        // Toggle the rotation state
        this.isImageRotated = !this.isImageRotated;
        const action = this.isSecondContainerVisible ? "closed" : "opened";
        this.publishToCommonChannel(action);
    }

    get secondContainerClass() {
        // Compute the class dynamically based on the visibility state
        return this.isSecondContainerVisible ? this.hiddenBlockLabel : this.visibleBlockLabel;
    }

    get imageClass() {
        // Compute the class dynamically based on the rotation state
        return this.isImageRotated ? this.rotationLabel : '';
    }


    transformData(incomingData) {
        let totalSections = incomingData.length;
        let passSections = incomingData.filter(section => section.auditStatus === this.passLabel).length;
    
        // this.passPercentage = (passSections / totalSections) * 100 + "%";
        this.passPercentage = Math.floor((passSections / totalSections) * 100) + this.percentageLabel;
    
        return incomingData.map((section, index) => {
            let icon;
            if (section.auditStatus === this.passLabel) {
                icon = RIGHT_ICON;
            } else if (section.auditStatus === this.failLabel) {
                icon = MINUS; 
            } else {
                icon = WARNING_ICON;
            }
    
            return {
                Id: (index + this.oneLabel).toString(),
                question: section.section,
                rotation: false,
                itemChangeStyle: false,
                icon: icon,
                counterQuestions: section.questions.map((question, qIndex) => {
                    return {
                        Id: (index + this.oneLabel).toString() + (qIndex + this.oneLabel).toString(),
                        questionText: question.question,
                        options: question.options.map(option => {
                            return {
                                label: option,
                                value: option,
                                checked: option === question.auditResponse,
                                disabled: true
                            };
                        })
                    };
                })
            };
        });
    }
    
    initializeProcessQuestion() {
        this.processQuestion = this.checklist;

        const firstQuestion = this.processQuestion[0];
        if (firstQuestion && firstQuestion.counterQuestions) {
            this.selectedCounterQuestions = firstQuestion.counterQuestions;
        }
    }

    @wire(CurrentPageReference)
    setCurrentPageReference(pageReference) {
        if (pageReference && pageReference.state && pageReference.state.c__isSubtab) {
            this.isSubtab = pageReference.state.c__isSubtab === 'true';
            console.log("Line no 125 === Sub Tab Detail :" + this.isSubtab);
        }
    }




    toggleRotation(event) {
        const rotationValue = event.currentTarget.dataset.rotation === 'true'; 
        console.log("Line no 218 == Rotation Value :" + rotationValue);
    
        const indexID = event.currentTarget.dataset.id; 
        console.log("line 220 == Index value :" + indexID);
    
        const clickedIndex = Number(event.currentTarget.dataset.index);
    
        this.checklist = this.checklist.map((item, index) => {
            return {
                ...item,
                rotation: index === clickedIndex ? !item.rotation : item.rotation
            };
        });
    
        const flipElement = this.template.querySelector('[' + this.dataIdLabel + '="' + indexID + '"]');
        if (flipElement) {
            flipElement.classList.toggle(this.rotationLabel); 
        }
    }
    

    questionHandler(event) {
        const clickedIndex = Number(event.currentTarget.dataset.index);
        this.processQuestion = this.processQuestion.map((item, index) => {
            if (index === clickedIndex) {
                this.selectedCounterQuestions = item.counterQuestions;
                return {
                    ...item,
                    rotation: !item.rotation
                };
            }
            return item;
        });
    }

    
    get iconClass() {
        return this.changeStyle ? this.rotationLabel : '';
    }

    openModal() {
        this.isShowModal = true;
    }

    hideModalBox() {
        this.isShowModal = false;
    }

 

    @api recordId;
    onOpenAccountClick() {
        
        if (!this.recordId) {
            console.error('No recordId available');
            return;
        }
        this.invokeWorkspaceAPI('isConsoleNavigation').then(isConsole => {
            if (isConsole) {
                console.log("Inside Is console condition");
                this.invokeWorkspaceAPI('getFocusedTabInfo').then(focusedTab => {
                    this.invokeWorkspaceAPI('openSubtab', {
                        parentTabId: focusedTab.tabId,
                        pageReference: {
                            type: this.navItemType,
                            attributes: {
                                apiName: this.autoAuditApiLable
                            },
                            state: {
                                c__recordId: this.recordId,
                                c__isSubtab: 'true'
                            }
                        },
                        focus: true
                    }).then(tabId => {
                        console.log("SubTab ID: ", tabId);
                        return this.invokeWorkspaceAPI('setTabLabel', {
                            tabId: tabId,
                            label: 'Auto-audit'
                        });
                    }).catch(error => {
                        console.error('Error opening subtab: ', error);
                    });
                });
            }
        });
    }
    
    invokeWorkspaceAPI(methodName, methodArgs) {
        return new Promise((resolve, reject) => {
            const apiEvent = new CustomEvent("internalapievent", {
                bubbles: true,
                composed: true,
                cancelable: false,
                detail: {
                    category: "workspaceAPI",
                    methodName: methodName,
                    methodArgs: methodArgs,
                    callback: (err, response) => {
                        if (err) {
                            reject(err);
                        } else {
                            resolve(response);
                        }
                    }
                }
            });
            window.dispatchEvent(apiEvent);
        });
    }
}

