/* Container styling for each component */
.auto-audit-box, .ai-wiki-box {
  border: 1px solid #e0e0e0;
  background-color: #fafafa;
  padding: 20px;
  margin: 20px 0;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.auto-audit-box h3, .ai-wiki-box h3 {
  font-size: 1.5rem;
  margin-bottom: 16px;
  color: #333;
}

/* Styling for each item block */
.auto-audit-item, .ai-wiki-item {
  display: flex;
  flex-direction: column;
  margin-bottom: 16px;
}

.auto-audit-item input, .ai-wiki-item input, .ai-wiki-item textarea {
  font-size: 0.9rem;
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  transition: border-color 0.2s;
}

.auto-audit-item input:focus, .ai-wiki-item input:focus, .ai-wiki-item textarea:focus {
  border-color: #007bff;
  outline: none;
}

/* Textarea styling */
.ai-wiki-item textarea {
  resize: vertical;
  min-height: 80px;
  max-height: 200px;
}

/* Button styling using classes */
.button-primary {
  font-size: 0.9rem;
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 8px 12px;
  cursor: pointer;
  border-radius: 4px;
  transition: background-color 0.3s, transform 0.2s;
}

.button-primary:hover {
  background-color: #0056b3;
  transform: translateY(-2px);
}

.button-primary:active {
  transform: translateY(0);
}

/* Add button styling for better spacing */
.auto-audit-box .button-primary, .ai-wiki-box .button-primary {
  margin-top: 8px;
  align-self: flex-start;
}

/* Responsive Design */
@media (max-width: 768px) {
  .auto-audit-box, .ai-wiki-box {
    padding: 16px;
  }

  .auto-audit-item, .ai-wiki-item {
    margin-bottom: 12px;
  }

  .button-primary {
    padding: 6px 10px;
  }
}
