{cases.map((caseItem, index) => (
  <div key={index} className="case-section">
    <input
      className="CallSummary-input-field"
      type="text"
      placeholder="Case ID"
      name="caseId"
      value={caseItem.caseId}
      onChange={(e) => handleCaseChange(index, e)}
    />
    <input
      className="CallSummary-input-field"
      type="date"
      placeholder="Creation Date"
      name="creationDate"
      value={caseItem.creationDate}
      onChange={(e) => handleCaseChange(index, e)}
    />
    <input
      className="CallSummary-input-field"
      type="text"
      placeholder="Subject"
      name="subject"
      value={caseItem.subject}
      onChange={(e) => handleCaseChange(index, e)}
    />
    <input
      className="CallSummary-input-field"
      type="text"
      placeholder="Priority"
      name="priority"
      value={caseItem.priority}
      onChange={(e) => handleCaseChange(index, e)}
    />
    <textarea
      className="CallSummary-input-field"
      placeholder="Description"
      name="description"
      value={caseItem.description}
      onChange={(e) => handleCaseChange(index, e)}
    />
    <button className="CallSummary-remove-button" onClick={() => handleRemoveCase(index)}>Remove Case</button>
  </div>
))}


const handleCaseChange = (index, e) => {
  const { name, value } = e.target;
  const updatedCases = cases.map((caseItem, i) =>
    i === index ? { ...caseItem, [name]: value } : caseItem
  );
  setCases(updatedCases);
};
