import { useState } from "react";
import { FaChevronDown, FaChevronUp } from "react-icons/fa6";

export default function Cases({ metadata, handleInput }) {
    const [tab, setTab] = useState(0)

    metadata['cases'] = metadata?.cases ? metadata?.cases : []

    function handleChangeCase(index, field, value) {
        let newCases = metadata?.cases
        newCases[index][field] = value
        handleInput('cases', newCases)
    }

    function handleChangeAtt(index, field, value, ind) {
        let newCases = metadata?.cases
        newCases[index][field][ind] = value
        handleInput('cases', newCases)
    }

    function handleChangeLinCom(index, field, ind, field2, value) {
        let newCases = metadata?.cases
        newCases[index][field][ind][field2] = value
        handleInput('cases', newCases)
    }

    function handleAddCase(e) {
        e.preventDefault()
        const newCase = ({
            'caseNumber': '',
            'priority': '',
            'creationDate': '',
            'subject': '',
            'description': '',
            'attachments': [],
            'linkedCases': [],
            'comments': [],
            'contactName': '',
            'status': '',
            'quickActions': '...'
        })

        const newCases = [...metadata?.cases, newCase]
        handleInput('cases', newCases)
        setTab(newCases.length - 1)
    }

    const handleCaseButtonClick = (index) => {
        if (tab === index) {
            setTab(null);
        } else {
            setTab(index)
        }
    }

    function handleAddAttachment(e, index) {
        e.preventDefault()
        let newCases = metadata?.cases
        newCases[index]['attachments'].push('')
        handleInput('cases', newCases)
    }

    function handleRemoveAttachment(e, index, ind) {
        e.preventDefault()
        let newCases = metadata?.cases
        newCases[index]['attachments'].splice(ind, 1)
        handleInput('cases', newCases)
    }

    function handleAddLinkedCase(e, index) {
        e.preventDefault()
        let newCases = metadata?.cases
        newCases[index]['linkedCases'].push({
            'caseNumber': '',
            'Subject': ''
        })
        handleInput('cases', newCases)
    }

    function handleRemoveLinkedCase(e, index, ind) {
        e.preventDefault()
        let newCases = metadata?.cases
        newCases[index]['linkedCases'].splice(ind, 1)
        handleInput('cases', newCases)
    }

    function handleAddComment(e, index) {
        e.preventDefault()
        let newCases = metadata?.cases
        newCases[index]['comments'].push({
            'date': '',
            'message': ''
        })
        handleInput('cases', newCases)
    }

    function handleRemoveComment(e, index, ind) {
        e.preventDefault()
        let newCases = metadata?.cases
        newCases[index]['comments'].splice(ind, 1)
        handleInput('cases', newCases)
    }

    function handleRemoveCase(e, index) {
        e.preventDefault()
        let newCases = metadata?.cases
        newCases.splice(index, 1)
        handleInput('cases', newCases)
        setTab(0)
    }

    const renderCaseDetails = (index) => (
        <div className="case-details">
            <div >
                <input
                    className='upload-demo-input'
                    placeholder="Case Number"
                    value={metadata?.cases[index]?.caseNumber}
                    onChange={(e) => handleChangeCase(index, 'caseNumber', e.target.value)}
                />
                <select
                    className='upload-demo-input'
                    value={metadata?.cases[index]?.priority}
                    onChange={(e) => handleChangeCase(index, 'priority', e.target.value)}
                >
                    <option hidden>Select Priority</option>
                    <option value='Low'>Low</option>
                    <option value='Medium'>Medium</option>
                    <option value='High'>High</option>
                </select>
                <label className="upload-demo-input" htmlFor={`creation-date-${index}`}>Creation Date</label>
                <input
                    className="upload-demo-input"
                    type="date"
                    id={`creation-date-${index}`}
                    placeholder="Creation Date"
                    value={metadata?.cases[index]?.creationDate}
                    onChange={(e) => handleChangeCase(index, 'creationDate', e.target.value)}
                />
                <div>
                    <input
                        className="upload-demo-subject-description"
                        placeholder="Subject"
                        value={metadata?.cases[index]?.subject}
                        onChange={(e) => handleChangeCase(index, 'subject', e.target.value)}
                    />
                </div>
                <div>
                    <textarea
                        className="upload-demo-subject-description"
                        placeholder="Description"
                        value={metadata?.cases[index]?.description}
                        onChange={(e) => handleChangeCase(index, 'description', e.target.value)}
                    ></textarea>
                </div>
                <div className="demo-section">
                    <b className="demo-section-child">Attachments</b>
                    <button className="demo-section-child demo-btn" onClick={e => handleAddAttachment(e, index)}>Add Attachment</button>
                    {metadata?.cases[index]?.attachments?.map((element, ind) => (
                        <div>
                            <input
                                className="upload-demo-input"
                                placeholder="Attachment Name"
                                value={metadata?.cases[index].attachments[ind]}
                                onChange={e => handleChangeAtt(index, 'attachments', e.target.value, ind)}
                            />
                            <button className="demo-btn remove-btn" onClick={e => handleRemoveAttachment(e, index, ind)}>Remove</button>
                        </div>
                    ))}
                </div>
                <div className="demo-section">
                    <b className="demo-section-child">Linked Cases</b>
                    <button className="demo-section-child demo-btn" onClick={e => handleAddLinkedCase(e, index)}>Add Linked Case</button>
                    {metadata?.cases[index]?.linkedCases?.map((element, ind) => (
                        <div>
                            <input
                                className="upload-demo-input"
                                placeholder="Case Number"
                                value={metadata?.cases[index].linkedCases[ind]['caseNumber']}
                                onChange={e => handleChangeLinCom(index, 'linkedCases', ind, 'caseNumber', e.target.value)}
                            />
                            <input
                                className="upload-demo-input"
                                placeholder="Subject"
                                value={metadata?.cases[index].linkedCases[ind]['Subject']}
                                onChange={e => handleChangeLinCom(index, 'linkedCases', ind, 'Subject', e.target.value)}
                            />
                            <button className="demo-btn remove-btn" onClick={e => handleRemoveLinkedCase(e, index, ind)}>Remove</button>
                        </div>
                    ))}
                </div>
                <div className="demo-section">
                    <b className="demo-section-child">Comments</b>
                    <button className="demo-section-child demo-btn" onClick={e => handleAddComment(e, index)}>Add Comment</button>
                    {metadata?.cases[index]?.comments?.map((element, ind) => (
                        <div>
                            <label htmlFor={`comment-date-${ind}`}>Date</label>
                            <input
                                className="upload-demo-input"
                                type='date'
                                placeholder="Date"
                                id={`comment-date-${ind}`}
                                value={metadata?.cases[index].comments[ind]['date']}
                                onChange={e => handleChangeLinCom(index, 'comments', ind, 'date', e.target.value)}
                            />
                            <input
                                className="upload-demo-input"
                                placeholder="Message"
                                value={metadata?.cases[index].comments[ind]['message']}
                                onChange={e => handleChangeLinCom(index, 'comments', ind, 'message', e.target.value)}
                            />
                            <button className="demo-btn remove-btn" onClick={e => handleRemoveComment(e, index, ind)}>Remove</button>
                        </div>
                    ))}
                </div>
            </div>
            <div>
                <input
                    className="upload-demo-input"
                    placeholder="Contact Name"
                    value={metadata?.cases[index]?.contactName}
                    onChange={(e) => handleChangeCase(index, 'contactName', e.target.value)}
                />
                <select
                    className="upload-demo-input"
                    value={metadata?.cases[index]?.status}
                    onChange={(e) => handleChangeCase(index, 'status', e.target.value)}
                >
                    <option hidden>Select Priority</option>
                    <option value='Open'>Open</option>
                </select>
            </div>
            <button className="demo-btn remove-btn" onClick={e => handleRemoveCase(e, index)}>Remove Case</button>
        </div>
    )

    return (
        <div className="demo-section-container">
            <div className="demo-btn-container">
                <button className="demo-btn" onClick={e => handleAddCase(e)}>Add New Case</button>
                {metadata?.cases?.map((item, index) => (
                    <div className="parent-case-header">
                        <div className="case-header">
                            <button className={`sub-btn ${tab === index ? 'active-sub-btn' : ''}`} onClick={() => handleCaseButtonClick(index)}> Case Details {index + 1} {tab === index ? <FaChevronDown /> : <FaChevronUp />} </button>
                        </div>
                    </div>
                ))}
            </div>
            <div className="case-details-container">
                {metadata?.cases?.length > 0 && tab !== null && (
                    <div className="case-container">
                        {renderCaseDetails(tab)}
                    </div>
                )}
            </div>
        </div >
    )
}
