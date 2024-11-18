     {selectedTab === 'Customer 360' && (
        <div className="customer-360-app">
          <h1>C360 View</h1>
          <Panel
            title="Risk Assessment"
            initialFields={[{ id: 1, name: "", value: "" }]}
            addFieldLabel="Add Field"
            fieldType="field"
          />
          <Panel
            title="Account Info"
            initialFields={[{ id: 2, name: "", value: "" }]}
            addFieldLabel="Add Sub Header"
            fieldType="field"
          />
          <Panel
            title="Next Best Action"
            initialFields={[{ id: 3, name: "Data Point 1", value: "" }]}
            addFieldLabel="Add Data Point"
            fieldType="data"
          />
          <Panel
            title="Life Events"
            initialFields={[{ id: 4, name: "Date", value: "" }]}
            addFieldLabel="Add Field"
            fieldType="field"
          />
          <Panel
            title="Marketing Analysis"
            initialFields={[{ id: 5, name: "", value: "" }]}
            addFieldLabel="Add Field"
            fieldType="field"
          />
        </div>
      )}
