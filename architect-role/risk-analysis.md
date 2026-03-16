# Risk Analysis

## Overview
Identifying risks early allows architects to address them before they become costly. Risk analysis is a proactive practice — not a reaction to failures.

## Core concepts

### Types of architectural risk
- **Technical risk**: unknown technologies, integration complexity, scalability limits.
- **Organizational risk**: unclear ownership, knowledge silos, team dependencies.
- **Business risk**: regulatory changes, shifting requirements, third-party dependencies.
- **Operational risk**: deployment complexity, observability gaps, missing runbooks.

### Risk assessment dimensions
For each risk, assess:
- **Likelihood**: how probable is this risk materializing?
- **Impact**: what is the consequence if it does?
- **Detectability**: how quickly would the team notice?

### Risk storming
Risk storming is a collaborative workshop technique to identify risks across a system design:

1. **Prepare**: share the architecture diagram with participants before the session.
2. **Individual identification**: each participant independently marks areas of concern on the diagram using sticky notes or color codes — without discussion.
3. **Group review**: present findings one by one. Identify areas where multiple participants flagged the same concern. These are high-priority risks.
4. **Prioritize**: rate each risk by likelihood and impact to focus mitigation efforts.
5. **Mitigate**: assign owners and define concrete mitigation steps or acceptance criteria.

Risk storming works best with a diverse group: developers, ops, product, and security, because different roles see different risks.

### Risk response strategies
- **Mitigate**: reduce likelihood or impact through design changes.
- **Accept**: acknowledge the risk and monitor it.
- **Transfer**: shift responsibility (e.g., use a managed service, insurance).
- **Avoid**: change the design to eliminate the risk entirely.

## Decision considerations / trade-offs
- Thorough risk analysis takes time but prevents expensive surprises late in delivery.
- Overengineering mitigations for low-probability risks wastes capacity.
- Risk storming requires psychological safety. participants must feel comfortable raising concerns.
