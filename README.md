<div className="CallSummary-tabs">
  <button
    className={`CallSummary-tab-button ${selectedTab === CUSTOMERDETAILS.CONTACT ? 'active-tab' : ''}`}
    onClick={() => setSelectedTab(CUSTOMERDETAILS.CONTACT)}
  >
    {CUSTOMERDETAILS.CONTACT}
  </button>
  <button
    className={`CallSummary-tab-button ${selectedTab === CUSTOMERDETAILS.CASES ? 'active-tab' : ''}`}
    onClick={() => setSelectedTab(CUSTOMERDETAILS.CASES)}
  >
    {CUSTOMERDETAILS.CASES}
  </button>
  <button
    className={`CallSummary-tab-button ${selectedTab === CUSTOMERDETAILS.INTERACTION_HISTORY ? 'active-tab' : ''}`}
    onClick={() => setSelectedTab(CUSTOMERDETAILS.INTERACTION_HISTORY)}
  >
    {CUSTOMERDETAILS.INTERACTION_HISTORY}
  </button>
  <button
    className={`CallSummary-tab-button ${selectedTab === CUSTOMERDETAILS.CUSTOMER_360 ? 'active-tab' : ''}`}
    onClick={() => setSelectedTab(CUSTOMERDETAILS.CUSTOMER_360)}
  >
    {CUSTOMERDETAILS.CUSTOMER_360}
  </button>
</div>
