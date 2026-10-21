# Introduction to Phishing

## Overview

This project documents my experience completing the [TryHackMe SOC Simulator - Introduction to Phishing](https://tryhackme.com/soc-sim/public-summary/aca9df485eb783415cbbe4a97e7df8f657999678bc4e53b5cee0586a51c4ffa4710b1edb813c3e5cc2825fbf0cf1afe4), a hands-on security operations exercise focused on investigating alerts, analyzing security events, and determining appropriate escalation and remediation actions.

Throughout the exercise, I investigated security alerts using the information provided in each case and supporting log data. My investigations included evaluating suspicious email activity, validating malicious indicators, correlating events across available logs, and documenting evidence-based incident assessments.

The objective of this project was to practice the investigative workflow of a security analyst, from initial alert review through classification, escalation, and recommended response.

## Investigation Workflow

My approach evolved throughout the simulator as I became more familiar with the available telemetry and recognized opportunities to strengthen my investigations.

1. **Alert Triage:** Reviewed each alert and its associated details to understand the reported activity, affected entities, and potential security implications.
2. **Evidence Collection:** Examined relevant email and firewall logs in Splunk to identify supporting events and investigate suspicious indicators.
3. **Log Correlation:** Compared timestamps, source and destination addresses, URLs, and other available event details to investigate related activity and determine whether connections were allowed or blocked.
4. **Alert Classification:** Evaluated the available evidence to determine whether the activity supported a true-positive classification.
5. **Escalation Assessment:** Identified unresolved questions, potential impacts, and circumstances requiring further investigation by the security team.
6. **Remediation Planning:** Documented recommended response actions based on the suspected attack, available evidence, and potential risk.

## Key Investigative Focus Areas

### Phishing and Malicious URL Investigation

Analyzed suspicious inbound email alerts for phishing indicators, including sender-domain inconsistencies, suspicious links, and messages designed to encourage recipients to take urgent action.

Used threat intelligence findings and firewall logs to investigate whether internal hosts had attempted to access suspicious destinations. Considered the implications of allowed versus blocked connections and identified follow-up investigations needed to assess potential exposure.

### Security Log Analysis

Used Splunk to examine available event records and investigate activity surrounding alert timestamps. Reviewed relevant fields to identify potentially related events and distinguish confirmed observations from assumptions requiring further validation.

### Incident Classification and Escalation

Practiced documenting the rationale behind alert classifications and identifying when additional investigation was necessary. Developed case reports covering affected entities, supporting evidence, attack indicators, escalation rationale, and recommended remediation actions.

## Tools and Skills

- **Splunk Enterprise:** Security log review, event investigation, and correlation of relevant activity.
- **Email and firewall logs:** Investigation of suspicious messages, URLs, network connections, and connection outcomes.
- **Threat intelligence:** Evaluation of suspicious destinations against available malicious-indicator information.
- **Incident triage:** Alert classification, evidence assessment, and escalation decisions.
- **Incident documentation:** Structured reporting, indicator identification, and remediation recommendations.

## Lessons Learned

One of the most important lessons from this exercise was recognizing the value of investigating beyond the initial alert details.

During my first three cases, I primarily focused on the information presented in each ticket. As I progressed, I began making fuller use of the available logs to investigate surrounding activity and look for corroborating evidence.

This improved my approach by helping me connect alert details with related events, evaluate whether suspicious activity was reflected in network logs, and identify additional questions that needed to be answered before drawing conclusions.

The exercise reinforced several important principles:

- **Correlate evidence across data sources.** An alert provides a starting point for an investigation, while supporting logs can help validate activity and establish context.
- **Distinguish observations from conclusions.** A malicious indicator or suspicious connection does not automatically establish that a host was compromised or credentials were stolen.
- **Investigate the scope and impact.** Identifying the affected entities and reviewing related activity helps determine what further investigation may be necessary.
- **Make remediation evidence-based.** Recommendations should reflect the observed activity, potential impact, and outstanding investigative questions.
- **Document findings clearly.** A useful case report communicates what is known, why the alert matters, and what actions should follow.

## Project Takeaways

This exercise strengthened my understanding of the SOC investigation lifecycle and the importance of combining alert context, log analysis, and threat intelligence to support security decisions.

It also reinforced the value of continuously refining an investigative methodology as familiarity with available tools and evidence improves.

## References

- [TryHackMe SOC Simulator — Public Summary](https://tryhackme.com/soc-sim/public-summary/aca9df485eb783415cbbe4a97e7df8f657999678bc4e53b5cee0586a51c4ffa4710b1edb813c3e5cc2825fbf0cf1afe4)
- [TryHackMe](https://tryhackme.com/)
- [Splunk Documentation](https://docs.splunk.com/)

*This project was completed in a controlled training environment. Findings and investigative conclusions are based on the evidence available within the simulator.*