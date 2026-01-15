Autonomous Lead Scoring workflow
  A high-performance automation workflow built in n8n that transforms raw inbound lead data into actionable sales intelligence. This system autonomously cleanses data, applies a weighted scoring algorithm, and routes prospects to the appropriate sales channels in real-time.📋 
  
Project Overview

  Manually vetting leads from form submissions is a significant bottleneck for sales teams. This project solves that by creating a "brain" between the intake form and the CRM/Communication tools. It ensures that high-value "Hot" leads are contacted within seconds, while lower-priority leads are nurtured without manual intervention.

Technical Architecture

  1. Data Normalization & Cleansing
  
  Incoming data from Google Sheets often contains inconsistencies (caps, extra spaces, varied headers).
  Case Normalization: Converts job titles and names to     lowercase to ensure logic consistency.
  Whitespace Trimming: Removes accidental trailing spaces common in form entries.Smart Key Lookup: A custom JavaScript function dynamically identifies column headers, ensuring the workflow doesn't break if a spreadsheet column is slightly renamed (e.g., "Company Size" vs " Company Size ").
  
  2. Proprietary Scoring Engine

   (JavaScript)The heart of the workflow is a weighted scoring node that evaluates leads based on Authority and Opportunity.
 Category    Criteria (Keywords) 
 
 PointsAuthority: CEO, Founder, Owner, Partner+Manager, Director, Head, VP
 
 Company Size: 51-200, 201-500++40Mid-MarketCompany Size: 11-50+20Small/StartupCompany Size: 1-10+5Logic 
 Output: 
 * Hot (70+): Decision makers at mid-to-large companies.
 * Warm (35-69): Decision makers at startups or mid-level managers.
 * Cold (<35): Low-level titles or very small teams.

  3. Intelligent Routing (The Router)

A logic-based Switch Node acts as the traffic controller, directing the JSON objects to different production paths based on the leadStatus variable.📊 

Business Impact & Results

Immediate Speed to Lead: Reduced response time for "Hot" leads from hours to under 30 seconds via real-time Slack alerts.

Team Efficiency: Sales teams no longer view "Cold" leads, allowing them to focus 100% of their energy on high-probability opportunities.

  5. 💻 Code Snippet: The Scoring LogicJavaScript

// Example of the logic used to differentiate leads


for (const item of $input.all()) {
  const jobTitle = item.json.jobTitle_clean.toLowerCase();
  const companySize = item.json.companySize_clean;
  
   let score = 0;
    // Weighted scoring for Authority
  if (jobTitle.includes('ceo') || jobTitle.includes('founder')) score += 50;
  // Weighted scoring for Opportunity
  if (companySize.includes('51-200')) score += 40;
  
  item.json.leadStatus = score >= 70 ? 'Hot' : 'Warm';
}
return $input.all();


⚙️ Setup & Installationn8n Environment: 

Import the provided .json workflow file.
Google Sheets: Connect your sheet using the "Append or Update" resource.
Slack API: Invite the n8n bot to the #lead-scoring channel.
Credentials: Set up OAuth2 for Google and Slack in the n8n credentials manager.
