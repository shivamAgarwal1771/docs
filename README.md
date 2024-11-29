import React, { useEffect } from 'react';
import { useDispatch } from 'react-redux';
import { updateDemoData } from "../../../store/demo-slice";
import { IoCloseSharp } from "react-icons/io5";

const Customer360 = ({ metadata, handleInput }) => {
    const dispatch = useDispatch();

    const validateRequiredFields = () => {
        if (metadata.customer360) {
            const customerDataa = Object.keys(metadata?.customer360);
            const requiredFields = new Set(['customerInfo', 'marketAnalysis', 'riskAssessment', 'nextBestAction', 'lifeEvents']);
            if (customerDataa.length !== 5) {
                requiredFields.forEach((obj, ind) => {
                    if (!(customerDataa[obj])) {
                        metadata.customer360[obj] = obj === 'marketAnalysis' && obj === 'riskAssessment' ? {} : []
                    }
                })
            }
        }
        return metadata.customer360;
    };

    let customer360data = metadata?.customer360 ? true : false;

    metadata['customer360'] = metadata?.customer360 ? validateRequiredFields() : {
        "customerInfo": [
            {
                title: [
                    { titleHeading: "Account Number", value: "12322435" }
                ],
                items: [
                    {
                        key: "Account Type",
                        value: "Savings"
                    },
                    {
                        key: "Balance",
                        value: "$2000"
                    },
                    {
                        key: "Last Transaction",
                        value: "$200 withdrawal, 14-02-24"
                    }
                ]
            }
        ],
        "marketAnalysis": {
            rates: {
                OpenRate: "30",
                ClickRate: "12"
            },
            summary: [
                {
                    description: "Responds well to email marketing."
                },
                {
                    description: "Has clicked on 3 FD schemes offers in app.",
                }
            ]
        },
        "nextBestAction": [
            {
                description: "Discuss the benefits of higher tier savings account."
            },
            {
                description: "Offer a home loan considering recent browsing session."
            }
        ],
        "riskAssessment": {
            summary: [
                {
                    description: "No late payments for the last 12 months."
                },
                {
                    description: "Low risk for default."
                }
            ],
            riskCategory: "Low"
        },
        "lifeEvents": [
            {
                date: "12/01/24",
                event: "Married"
            },
            {
                date: "24/12/23",
                event: "New Car"
            },
            {
                date: "12/03/21",
                event: "Graduated"
            }
        ],
    }

    useEffect(() => {
        // This will automatically trigger the click of the "Add" button after 2 seconds
        const timer = setTimeout(() => {
            const addButton = document.querySelector('.add-life-event-btn');
            if (addButton) {
                addButton.click(); // Simulate button click
            }
        }, 2000); // Wait for 2 seconds before clicking

        return () => clearTimeout(timer); // Cleanup on component unmount
    }, []);

    function handleAdd(e, type, subType) {
        e.preventDefault();
        let newCustomer360 = metadata.customer360;

        if (type === 'customerInfo') {
            if (type === 'customerInfo' && subType === 'items') {
                newCustomer360.customerInfo = newCustomer360.customerInfo.map(info => ({
                    ...info,
                    items: [...(info.items || []), { key: "", value: "" }]
                }));
            } else {
                newCustomer360.customerInfo = [...(newCustomer360.customerInfo || []), { title: [{ titleHeading: "", value: "" }], items: [{ key: "", value: "" }], }];
            }
        }
        else if (type === 'marketAnalysis' && subType === 'rates') {
            newCustomer360.marketAnalysis = { ...(newCustomer360.marketAnalysis || {}), rates: { ...(newCustomer360.marketAnalysis.rates || {}), OpenRate: "", ClickRate: "" } }
        } else if (type === 'marketAnalysis' && subType === 'summary') {
            newCustomer360.marketAnalysis = { ...(newCustomer360.marketAnalysis || {}), summary: [...(newCustomer360.marketAnalysis.summary || []), { description: "" }] }
        } else if (type === 'riskAssessment' && subType === 'summary') {
            newCustomer360.riskAssessment = { ...(newCustomer360.riskAssessment || {}), summary: [...(newCustomer360.riskAssessment.summary || []), { description: "" }] }
        } else if (type === 'riskAssessment' && subType === 'riskCategory') {
            newCustomer360.riskAssessment = { ...(newCustomer360.riskAssessment || {}), riskCategory: "" }
        } else if (type === 'nextBestAction') {
            newCustomer360.nextBestAction = [...(newCustomer360.nextBestAction || []), { description: "" }]
        } else if (type === 'lifeEvents') {
            newCustomer360.lifeEvents = [...(newCustomer360.lifeEvents || []), { date: "", event: "" }]
        }
        handleInput('customer360', newCustomer360);
    }

    function handleChange(index, type, value, subType, subIndex, subKey) {
        let newCustomer360 = metadata.customer360;

        if (type === 'customerInfo') {
            if (subType === 'title') {
                newCustomer360.customerInfo[index].title[subIndex][subKey] = value;
            } else if (subType === 'items') {
                newCustomer360.customerInfo[index].items[subIndex][subKey] = value;
            }
        } else if (type === 'marketAnalysis' && subType === 'rates') {
            newCustomer360.marketAnalysis.rates[subKey] = value;
        } else if (type === 'marketAnalysis' && subType === 'summary') {
            newCustomer360.marketAnalysis.summary[index][subKey] = value;
        } else if (type === 'riskAssessment' && subType === 'summary') {
            newCustomer360.riskAssessment.summary[index][subKey] = value;
        } else if (type === 'riskAssessment' && subType === 'riskCategory') {
            newCustomer360.riskAssessment[subType] = value;
        } else if (type === 'nextBestAction') {
            newCustomer360.nextBestAction[index][subType] = value;
        } else if (type === 'lifeEvents') {
            newCustomer360.lifeEvents[index][subType] = value;
        }
        handleInput('customer360', newCustomer360);
    }

    function handleRemove(e, type, index, subType, subIndex, subKey) {
        e.preventDefault();
        let newCustomer360 = metadata.customer360;

        if (type === 'customerInfo') {
            if (subType === 'title') {
                newCustomer360.customerInfo[index].title.splice(subIndex, 1);
            } else if (subType === 'items') {
                newCustomer360.customerInfo[index].items.splice(subIndex, 1);
            } else {
                newCustomer360.customerInfo.splice(index, 1);
            }
        } else if (type === 'marketAnalysis' && subType === 'rates') {
            const { [subKey]: _, ...remainingRates } = newCustomer360.marketAnalysis.rates;
            newCustomer360.marketAnalysis.rates = remainingRates;
        } else if (type === 'marketAnalysis' && subType === 'summary') {
            newCustomer360.marketAnalysis.summary.splice(index, 1);
        } else if (type === 'riskAssessment' && subType === 'summary') {
            newCustomer360.riskAssessment.summary.splice(index, 1);
        } else if (type === 'nextBestAction') {
            newCustomer360.nextBestAction.splice(index, 1);
        } else if (type === 'lifeEvents') {
            newCustomer360.lifeEvents.splice(index, 1);
        }
        handleInput('customer360', newCustomer360);
    }

    return (
        <div className="customer360-container field-column padding-10 gap-10">
            {/* Other sections remain unchanged */}

            <div className="border-box rounded-border">
                <div className="demo-section">
                    <span className="customer360-header"><h1><b>Life Events</b></h1></span>
                    <button className="add-fields-btn add-life-event-btn" onClick={e => handleAdd(e, 'lifeEvents')}>+ Add</button>
                </div>
                {metadata.customer360.lifeEvents?.map((item, index) => (
                    <div className='personal-info-field-value-child padding-10' key={index}>
                        <input
                            className='upload-demo-input left-curved-input call-info-input-width'
                            placeholder='Date'
                            value={item.date}
                            onChange={e => handleChange(index, 'lifeEvents', e.target.value, 'date')}
                        />
                        <input
                            className='upload-demo-input call-info-input-width'
                            placeholder='Event'
                            value={item.event}
                            onChange={e => handleChange(index, 'lifeEvents', e.target.value, 'event')}
                        />
                        <button
                            className='remove-btn'
                            onClick={e => handleRemove(e, 'lifeEvents', index)}
                        >
                            <IoCloseSharp />
                        </button>
                    </div>
                ))}
            </div>
        </div>
    );
};

export default Customer360;
