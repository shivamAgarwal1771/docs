const handleInteractionChange = (index, e) => {
  const { name, value } = e.target;
  const updatedInteractions = interactions.map((interaction, i) =>
    i === index ? { ...interaction, [name]: value } : interaction
  );
  setInteractions(updatedInteractions);
};

{interactions.map((interaction, index) => (
  <div key={index} className="CallSummary-interaction-section">
    <input
      className="CallSummary-input-field"
      type="text"
      placeholder="Title"
      name="title"
      value={interaction.title}
      onChange={(e) => handleInteractionChange(index, e)}
    />
    <input
      className="CallSummary-input-field"
      type="date"
      placeholder="Date"
      name="date"
      value={interaction.date}
      onChange={(e) => handleInteractionChange(index, e)}
    />
    <input
      className="CallSummary-input-field"
      type="time"
      placeholder="Time"
      name="time"
      value={interaction.time}
      onChange={(e) => handleInteractionChange(index, e)}
    />
    <textarea
      className="CallSummary-input-field"
      placeholder="Description"
      name="description"
      value={interaction.description}
      onChange={(e) => handleInteractionChange(index, e)}
    />
    <button className="CallSummary-remove-button" onClick={() => handleRemoveInteraction(index)}>Remove Interaction</button>
  </div>
))}
