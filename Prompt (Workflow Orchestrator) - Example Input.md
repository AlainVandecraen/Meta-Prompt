# USER OBJECTIVE

I want to build an automated email system for our e-commerce webshop that analyzes incoming customer complaints. The system needs to determine if a customer is eligible for a refund based on our return policy, assess the customer's tone (angry, frustrated, neutral), and automatically draft a personalized response concept (draft email) for our helpdesk agents. If the customer is extremely angry, the email must immediately escalate to a manager. It must be highly reliable and should never promise a refund without a double check.

# USER PRIMARY INPUTS

RAW_CUSTOMER_EMAIL:

"Dear webshop, I bought a jacket from you last week (order #9482) but the zipper broke immediately when I put it on. This is really garbage quality for that price!! I want my money back right away or a new jacket, and I am not paying for the return shipping. I've had the jacket for 6 days now. Let me hear from you quickly because I need that jacket this weekend for a wedding."

STORE_RETURN_POLICY:

- Customers have a 14-day right to return purchases without giving a reason.
- Products must be undamaged and unworn (trying them on is, of course, allowed).
- If a product is delivered defective (manufacturing defect), the webshop covers the return shipping costs and the customer is entitled to a free replacement or a full refund.
- Damage caused by the customer after use is excluded from refunds.

# USER SECONDARY INPUTS

BRAND_VOICE_GUIDELINES:

- Tone: Empathetic, professional, solution-oriented, and always polite.
- Never react defensively, even if the customer is rude.
- Always address the customer formally using 'u' (or appropriate formal pronouns) and 'Dear `[Name]`'.
- Close with a standard professional sign-off and the name of the support team.

ESCALATION_CRITERIA:

- Sentiment score: If the sentiment is 'Critical/Extremely Frustrated', add the tag `[MANAGER_ESCALATION]` to the email's metadata.
- Financial risk: Claims exceeding €250 are always escalated.
