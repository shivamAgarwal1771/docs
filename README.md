import React, { useEffect } from 'react';
import { useDispatch } from 'react-redux';
import { updateDemoData } from "../../../store/demo-slice";
import { IoCloseSharp } from "react-icons/io5";

const Customer360 = ({ metadata, handleInput }) => {
    const dispatch = useDispatch()

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

    let customer360data = metadata?.customer360 ? true : false

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
        if (!customer360data) {
            dispatch(updateDemoData({ type: "metadata", data: metadata }))
        };
    }, [customer360data])

    function handleAdd(e, type, subType) {
        e.preventDefault()
        let newCustomer360 = metadata.customer360

        if (type === 'customerInfo') {
            if (type == 'customerInfo' && subType === 'items') {
                newCustomer360.customerInfo = newCustomer360.customerInfo.map(info => ({
                    ...info,
                    items: [...(info.items || []), { key: "", value: "" }]
                }))
            } else {
                newCustomer360.customerInfo = [...(newCustomer360.customerInfo || []), { title: [{ titleHeading: "", value: "" }], items: [{ key: "", value: "" }], }]
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
        handleInput('customer360', newCustomer360)
    }

    function handleChange(index, type, value, subType, subIndex, subKey) {
        let newCustomer360 = metadata.customer360

        if (type === 'customerInfo') {
            if (subType === 'title') {
                newCustomer360.customerInfo[index].title[subIndex][subKey] = value;
            } else if (subType === 'items') {
                newCustomer360.customerInfo[index].items[subIndex][subKey] = value;
            }
            // newCustomer360.customerInfo[index][subType] = value;
        } else if (type === 'marketAnalysis' && subType === 'rates') {
            newCustomer360.marketAnalysis.rates[subKey] = value
        } else if (type === 'marketAnalysis' && subType === 'summary') {
            newCustomer360.marketAnalysis.summary[index][subKey] = value
        } else if (type === 'riskAssessment' && subType === 'summary') {
            newCustomer360.riskAssessment.summary[index][subKey] = value
        } else if (type === 'riskAssessment' && subType === 'riskCategory') {
            newCustomer360.riskAssessment[subType] = value
        } else if (type === 'nextBestAction') {
            newCustomer360.nextBestAction[index][subType] = value
        } else if (type === 'lifeEvents') {
            newCustomer360.lifeEvents[index][subType] = value
        }
        handleInput('customer360', newCustomer360)
    }

    function handleRemove(e, type, index, subType, subIndex, subKey) {
        e.preventDefault()
        let newCustomer360 = metadata.customer360

        if (type === 'customerInfo') {
            if (subType === 'title') {
                newCustomer360.customerInfo[index].title.splice(subIndex, 1)
            } else if (subType === 'items') {
                newCustomer360.customerInfo[index].items.splice(subIndex, 1)
            } else {
                newCustomer360.customerInfo.splice(index, 1)
            }
        } else if (type === 'marketAnalysis' && subType === 'rates') {
            const { [subKey]: _, ...remainingRates } = newCustomer360.marketAnalysis.rates
            newCustomer360.marketAnalysis.rates = remainingRates
        } else if (type === 'marketAnalysis' && subType === 'summary') {
            newCustomer360.marketAnalysis.summary.splice(index, 1)
        } else if (type === 'riskAssessment' && subType === 'summary') {
            newCustomer360.riskAssessment.summary.splice(index, 1)
        } else if (type === 'nextBestAction') {
            newCustomer360.nextBestAction.splice(index, 1)
        } else if (type === 'lifeEvents') {
            newCustomer360.lifeEvents.splice(index, 1)
        }
        handleInput('customer360', newCustomer360)
    }

    return (
        <div className="customer360-container field-column padding-10 gap-10">
            <div className="border-box rounded-border">
                <div className="demo-section">
                    <span className="customer360-header"><h1><b>Customer Info</b></h1></span>
                    <button className="add-fields-btn" onClick={e => handleAdd(e, 'customerInfo')}>+ Add</button>
                </div>
                {metadata?.customer360?.customerInfo?.map((item, index) => (
                    <div key={index}>
                        <div className='customer-info-items padding-2vh align-item-center'>
                            <span className="font-14 width-15"><b>Title :</b></span>
                            <div className="personal-info-field-value width-85">
                                {item?.title?.map((title, titleIndex) => (
                                    <div className='personal-info-field-value-child' key={titleIndex}>
                                        <input
                                            className='upload-demo-input left-curved-input call-info-input-width'
                                            placeholder='Field'
                                            value={title.titleHeading}
                                            onChange={e => handleChange(index, 'customerInfo', e.target.value, 'title', titleIndex, 'titleHeading')}
                                        />
                                        <input
                                            className='upload-demo-input call-info-input-width'
                                            placeholder='Value'
                                            value={title.value}
                                            onChange={e => handleChange(index, 'customerInfo', e.target.value, 'title', titleIndex, 'value')}
                                        />
                                        <button className='section-remove-btn' onClick={e => handleRemove(e, 'customerInfo', index, 'title', titleIndex)}><IoCloseSharp className="close-btn-icon" /></button>
                                    </div>
                                ))}
                            </div>
                        </div>
                        <div className='customer-info-items padding-2vh align-item-top'>
                            <span className="font-14 width-15"><b>Personal Info :</b></span>
                            <div className="personal-info-field-value width-85 gap-10">
                                {item?.items?.map((title, titleIndex) => (
                                    <div className='personal-info-field-value-child' key={titleIndex}>
                                        <input
                                            className='upload-demo-input left-curved-input call-info-input-width'
                                            placeholder='Field'
                                            value={title.key}
                                            onChange={e => handleChange(index, 'customerInfo', e.target.value, 'items', titleIndex, 'key')}
                                        />
                                        <input
                                            className='upload-demo-input call-info-input-width'
                                            placeholder='Value'
                                            value={title.value}
                                            onChange={e => handleChange(index, 'customerInfo', e.target.value, 'items', titleIndex, 'value')}
                                        />
                                        <button className='section-remove-btn' onClick={e => handleRemove(e, 'customerInfo', index, 'items', titleIndex)}><IoCloseSharp className="close-btn-icon" /></button>
                                    </div>
                                ))}
                                <div className='field-row padding-10 gap-10'>
                                    <button className='add-fields-btn rounded-border call-info-input-width' onClick={e => handleAdd(e, 'customerInfo', 'items')}> Add more Items </button>
                                    <button className='section-remove-btn rounded-border call-info-input-width' onClick={e => handleRemove(e, 'customerInfo', index)}> Remove </button>
                                </div>
                            </div>
                        </div>
                        {/* <button className='demo-btn remove-btn' onClick={e => handleRemove(e, 'customerInfo', index)}> Remove </button> */}
                    </div>
                ))}
            </div>

            <div className="border-box rounded-border">
                <div className="demo-section">
                    <span className="customer360-header"><h1><b>Market Analysis Rates</b></h1></span>
                    <button className="add-fields-btn" onClick={e => handleAdd(e, 'marketAnalysis', 'rates')}>+ Add</button>
                </div>
                {metadata?.customer360?.marketAnalysis?.rates && Object.entries(metadata.customer360.marketAnalysis.rates).map(([key, value], index) => (
                    <div className='personal-info-field-value-child padding-10' key={index}>
                        <input
                            className='upload-demo-input left-curved-input call-info-input-width'
                            placeholder='Rate Type'
                            value={key}
                            // onChange={e => handleChange(index, 'customerInfo', e.target.value, 'key')}
                            readOnly
                        />
                        <input
                            className='upload-demo-input call-info-input-width'
                            placeholder='Value'
                            value={value}
                            onChange={e => handleChange(index, 'marketAnalysis', e.target.value, 'rates', index, key)}
                        />
                        <button className='section-remove-btn' onClick={e => handleRemove(e, 'marketAnalysis', index, 'rates', key)}><IoCloseSharp className="close-btn-icon" /></button>
                    </div>
                ))}
            </div>

            <div className="border-box rounded-border">
                <div className="demo-section">
                    <span className="customer360-header"><h1><b>Market Analysis Summary</b></h1></span>
                    <button className="add-fields-btn" onClick={e => handleAdd(e, 'marketAnalysis', 'summary')}>+ Add</button>
                </div>
                {metadata?.customer360?.marketAnalysis?.summary?.map((item, index) => (
                    <div className='personal-info-field-value-child padding-10' key={index}>
                        <input
                            className='upload-demo-input left-curved-input width-100'
                            placeholder='Description'
                            value={item.description}
                            onChange={e => handleChange(index, 'marketAnalysis', e.target.value, 'summary', index, 'description')}
                        />
                        <button className='section-remove-btn' onClick={e => handleRemove(e, 'marketAnalysis', index, 'summary', index)}><IoCloseSharp className="close-btn-icon" /></button>
                    </div>
                ))}
            </div>

            <div className="border-box rounded-border">
                <div className="demo-section">
                    <span className="customer360-header"><h1><b>Risk Assessment Summary</b></h1></span>
                    <button className="add-fields-btn" onClick={e => handleAdd(e, 'riskAssessment', 'summary')}>+ Add</button>
                </div>
                {metadata?.customer360?.riskAssessment?.summary?.map((item, index) => (
                    <div className='personal-info-field-value-child padding-10' key={index}>
                        <input
                            className='upload-demo-input left-curved-input width-100'
                            placeholder='Description'
                            value={item.description}
                            onChange={e => handleChange(index, 'riskAssessment', e.target.value, 'summary', index, 'description')}
                        />
                        <button className='section-remove-btn' onClick={e => handleRemove(e, 'riskAssessment', index, 'summary', index)}><IoCloseSharp className="close-btn-icon" /></button>
                    </div>
                ))}
            </div>

            <div className="border-box rounded-border">
                <div className="demo-section">
                    <span className="customer360-header"><h1><b>Risk Category</b></h1></span>
                    <select
                        className='upload-demo-input rounded-border call-info-input-width'
                        value={metadata?.customer360?.riskAssessment?.riskCategory}
                        onChange={e => handleChange(null, 'riskAssessment', e.target.value, 'riskCategory')}
                    >
                        <option value={'High'}>High </option>
                        <option value={'Medium'}>Medium</option>
                        <option value={'Low'}>Low</option>
                    </select>
                </div>
            </div>

            <div className="border-box rounded-border">
                <div className="demo-section">
                    <span className="customer360-header"><h1><b>Next Best Action</b></h1></span>
                    <button className="add-fields-btn" onClick={e => handleAdd(e, 'nextBestAction')}>+ Add</button>
                </div>
                {metadata?.customer360?.nextBestAction?.map((item, index) => (
                    <div className='personal-info-field-value-child padding-10' key={index}>
                        <input
                            className='upload-demo-input left-curved-input width-100'
                            placeholder='Description'
                            value={item.description}
                            onChange={e => handleChange(index, 'nextBestAction', e.target.value, 'description')}
                        />
                        <button className='section-remove-btn' onClick={e => handleRemove(e, 'nextBestAction', index)}><IoCloseSharp className="close-btn-icon" /></button>
                    </div>
                ))}
            </div>

            <div className="border-box rounded-border">
                <div className="demo-section">
                    <span className="customer360-header"><h1><b>Life Events</b></h1></span>
                    <button className="add-fields-btn" onClick={e => handleAdd(e, 'lifeEvents')}>+ Add</button>
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
                        <button className='section-remove-btn' onClick={e => handleRemove(e, 'lifeEvents', index)}><IoCloseSharp className="close-btn-icon" /></button>
                    </div>
                ))}
            </div>
        </div>
    )
}

export default Customer360
