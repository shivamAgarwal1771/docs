<select
  className="CallSummary-input-field call-info-input-width rounded-border"
  name="priority"
  value={caseItem.priority}  // The selected value is tied to caseItem.priority
  onChange={(e) => handleCaseChange(index, e)}  // handleCaseChange receives the event
>
  <option value="">Select Priority</option>
  <option value="Low">Low</option>
  <option value="Mid">Mid</option>
  <option value="High">High</option>
</select>
