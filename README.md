import { useEffect } from "react";
import { useDispatch } from "react-redux";
import { updateDemoData } from "../../../store/demo-slice";

export default function CallSummary({ metadata, handleInput , handleSubmit }) {
    const dispatch = useDispatch()
    const validateRequiredSummaryFields = () => {
        if (metadata.summary) {
            const summary = Object.keys(metadata?.summary);
            const requiredFields = new Set(['callSummary', 'callNotes', 'resolution']);
            if (summary.length !== 3) {
                requiredFields.forEach((obj, ind) => {
                    if (!(summary[obj])) {
                        metadata.summary[obj] = []
                    }
                })
            }
        }
        return metadata.summary;
    };

    let summarydata = metadata?.summary ? true : false

    metadata['summary'] = metadata?.summary ? validateRequiredSummaryFields() : {
        'callSummary': [
            { key: "Intent Captured", value: "" },
            { key: "Repeat Call", value: "No" },
            { key: "Incoming Time", value: "03:15 PM, 13-07-2024" },
            { key: "Call Duration", value: "1:18 mins" }
        ],
        'callNotes': [],
        'resolution': [
            {
                QUESTION: "Was the captured intent correct?",
                OPTIONS: [
                    {
                        value: "Yes",
                        selected: true
                    },
                    {
                        value: "No",
                        selected: false
                    }
                ]
            },
            {
                QUESTION: "Was the issue resolved?",
                OPTIONS: [
                    {
                        value: "Yes",
                        selected: true
                    },
                    {
                        value: "No",
                        selected: false
                    }
                ]
            },
            {
                QUESTION: "Is any follow-up needed?",
                OPTIONS: [
                    {
                        value: "Yes",
                        selected: false
                    },
                    {
                        value: "No",
                        selected: true
                    }
                ]
            }
        ]
    };

    useEffect(() => {
        if (!summarydata) { 
            dispatch(updateDemoData({ type: "metadata", data: metadata })) 
        };
    }, [summarydata])

    function handleAdd(e, type) {
        e.preventDefault()
        let newSummary = metadata.summary

        if (type === 'callSummary') {
            newSummary[type].push({ key: "", value: "" })
        }

        if (type === 'callNotes') {
            newSummary[type].push({ description: '' })
        }

        if (type === 'resolution') {
            newSummary[type].push({
                'QUESTION': '',
                'OPTIONS': []
            })
        }

        handleInput('summary', newSummary)
    }

    function handleAddOption(e, index) {
        e.preventDefault()
        let newSummary = metadata.summary
        newSummary?.resolution[index]?.OPTIONS?.push({
            value: '',
            selected: false
        })
        handleInput('summary', newSummary)
    }

    function handleCallInfoChange(index, type, value) {
        let newSummary = metadata.summary
        newSummary.callSummary[index][type] = value
        handleInput('summary', newSummary)
    }

    function handleCallNotesChange(index, value) {
        let newSummary = metadata.summary
        newSummary.callNotes[index].description = value
        handleInput('summary', newSummary)
    }

    function handleResolutionChange(index, type, value, optInd = undefined, optType = undefined) {
        let newSummary = metadata.summary

        console.log(newSummary, index, type, optInd, optType, value)

        if (optInd === undefined) {
            newSummary.resolution[index][type] = value
        } else {
            newSummary.resolution[index][type][optInd][optType] = value
        }

        handleInput('summary', newSummary)
    }

    function handleRemove(e, type, index) {
        e.preventDefault()
        let newSummary = metadata.summary

        newSummary[type].splice(index, 1)

        handleInput('summary', newSummary)
    }

    function handleRemoveOption(e, index, optIndex) {
        e.preventDefault()
        let newSummary = metadata.summary

        newSummary.resolution[index].OPTIONS.splice(optIndex, 1)

        handleInput('summary', newSummary)
    }

    return (
        <>
            <div className="demo-section"><span className="demo-section-child">Call Info:</span><button className="demo-section-child demo-btn" onClick={e => handleAdd(e, 'callSummary')}>Add </button></div>
            {metadata?.summary?.callSummary?.map((item, index) => (
                <div className="section-child">
                    <input
                        className="upload-demo-input"
                        placeholder="Field"
                        value={item.key}
                        onChange={e => handleCallInfoChange(index, 'key', e.target.value)}
                    />
                    <input
                        className="upload-demo-input"
                        placeholder="Value"
                        value={item.value}
                        onChange={e => handleCallInfoChange(index, 'value', e.target.value)}
                    />
                    <button className="section-btn section-remove-btn" onClick={e => handleRemove(e, 'callSummary', index)}>Remove</button>
                </div>
            ))}
            <div className="demo-section"><span className="demo-section-child">Call Notes:</span><button className="demo-section-child demo-btn" onClick={e => handleAdd(e, 'callNotes')}>Add</button></div>
            {metadata?.summary?.callNotes?.map((item, index) => (
                <div>
                    <input
                        className="upload-demo-input"
                        placeholder="Add Notes"
                        value={item?.description}
                        onChange={e => handleCallNotesChange(index, e.target.value)}
                    />
                    <button className="demo-btn remove-btn" onClick={e => handleRemove(e, 'callNotes', index)}>Remove</button>
                </div>
            ))}
            <div className="demo-section"><span className="demo-section-child">Resolution:</span><button className="demo-section-child demo-btn" onClick={e => handleAdd(e, 'resolution')}>Add</button></div>
            {metadata?.summary?.resolution?.map((item, index) => (
                <>
                    <div className="demo-section">
                        <span>Question</span>
                        <input
                            className="upload-demo-subject-description"
                            placeholder="Question"
                            value={item.QUESTION}
                            onChange={e => handleResolutionChange(index, 'QUESTION', e.target.value)}
                        />
                        <button className="demo-btn remove-btn" onClick={e => handleRemove(e, 'resolution', index)}>Remove</button>
                    </div>
                    <button className="demo-btn" onClick={e => handleAddOption(e, index)}>Add Option</button>
                    {item?.OPTIONS?.map((opt, ind) => (
                        <div className="demo-section">
                            <span>
                                Option
                                <input
                                    className="upload-demo-input"
                                    placeholder="Option"
                                    value={opt.value}
                                    onChange={e => handleResolutionChange(index, 'OPTIONS', e.target.value, ind, 'value')}
                                />
                            </span>
                            <span>
                                Selected Option
                                <input
                                    type="checkbox"
                                    value={opt.selected}
                                    checked={opt.selected}
                                    onChange={e => handleResolutionChange(index, 'OPTIONS', e.target.checked, ind, 'selected')}
                                />
                            </span>
                            <button className="demo-btn remove-btn" onClick={e => handleRemoveOption(e, index, ind)}>Remove</button>
                        </div>
                    ))}
                </>
            ))}
            <button type="submit" onClick={handleSubmit}>Submit</button>
        </>
    )
}
