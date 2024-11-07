/* Tabs Layout */
.tabs-container {
   display: flex;
   border-bottom: 2px solid #ccc;
   padding: 0 10px;
   gap: 20px;
}

.tab {
   padding: 12px 16px;
   cursor: pointer;
   transition: color 0.3s ease;
   font-weight: 500;
}

.tab.active {
   color: #4a90e2;
   border-bottom: 3px solid #4a90e2;
}

/* Content Sections */
.content-section {
   padding: 20px;
   background-color: #f9f9f9;
   border-radius: 8px;
   box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
   margin-top: 20px;
}

/* Buttons and Interactive Elements */
.button {
   padding: 10px 20px;
   border: none;
   border-radius: 4px;
   background-color: #4a90e2;
   color: #fff;
   cursor: pointer;
   font-size: 14px;
   transition: background-color 0.3s ease;
}

.button:hover {
   background-color: #357ab3;
}

/* Spacing and Layout Consistency */
.section-container {
   margin-bottom: 30px;
}

/* Typography */
.section-heading {
   font-size: 18px;
   font-weight: bold;
   color: #333;
   margin-bottom: 10px;
}

.section-text {
   font-size: 14px;
   color: #666;
}
