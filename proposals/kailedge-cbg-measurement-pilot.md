# Development Fund Proposal
Author: Prasad Gopal, Founder, KAILEdge
Status: Draft
Created: October 2026
Label: financial-workflows-composability
Champion: Parth Chaturvedi (Canton Foundation)
RFP Category: Primary: RWA Standards (RFP 12), area "Daml and Institutional RWA Workflow Standards". Secondary: Public verifiability (RFP 11). If the committee considers the work outside the RFPs, we ask that it be considered as an individual initiative.

## Abstract
This proposal funds Phase 1 of a proposed pilot at a Compressed Biogas (CBG) facility at Harohalli, Karnataka, India, operated by Sustainable Impacts. The pilot would build a reference integration from device-sealed plant measurements to Daml contracts on Canton, and release the open-source integration layer.
The total funding request is $120,000 across four tranches. The pilot is planned to run one full production cycle (about 5 months). Dates depend on a signed host-site agreement.

## Specification
### 1. Objective
Problem statement: Carbon credit verification is retrospective. Physical production is continuous; evidence is assembled after the fact from fragmented telemetry, batch sampling and modelling assumptions.
India's CBG sector: 217 plants functional as of 31 July 2026 (https://www.thehindubusinessline.com/markets/commodities/india-achieves-105-cbg-blending-in-fy26-ahead-of-1-target/article71314065.ece); SATAT plans 5,000 plants by 2030 (https://pngrb.gov.in/pdf/confluence/SESSION-8-CBG-CELL.pdf).

Intended outcome: demonstrate a hardware-anchored alternative to manual, retrospective measurement, built from:
1. A deterministic physics model running at the edge (design: 62 cross-coupled cellular automata across 21 coupled physical domains)
2. TPM 2.0 hardware-sealed certificates (design: 128-byte records)
3. Daml smart contracts on Canton that verify certificates and support measurement-backed asset workflows
The single objective is to show that this pipeline can run in field conditions and be consumed on Canton, and to leave a reusable open reference implementation. KAILEdge is a monitoring and verification layer. It does not issue or manage carbon credits.

### 2. Implementation Mechanics
Three-layer architecture at the Harohalli CBG facility. Sensing is read-only: no actuators and no connection to plant control systems.
Layer 1 - Instrumentation: PT100 temperature probes (digester, gas line, ambient); pressure transmitters (gas line, compression); pH and ORP electrodes (digester health); load-cell volumetric station (methane flow reference); NDIR CH4/CO2 sensors and H2S detectors (trend monitoring); MEMS vibration sensors (equipment health); Modbus energy meters (subsystem power monitoring); feedstock mass and composition measurement.
Measurement classes: Class A reference sensors, calibrated by an NABL-accredited laboratory, used to validate the physics model. Class B operational telemetry, continuous monitoring, not certified. Class C derived quantities computed by the physics engine.
Layer 2 - Deterministic physics: 62 cross-coupled cellular automata; 21 physical domains coupled through a 21x21 matrix; 50+ non-linear equations. Target: combined methane mass uncertainty <=8% during the pilot, with a path to <=4% with daily GC and dual NDIR. These are targets, not measured results.
Layer 3 - Hardware seal: state vector sealed in TPM 2.0 (PCR 12 extension, SHA3-256 hash, device-bound signature, hardware-anchored identity). Output: a 128-byte certificate designed to be tamper-evident and independently replayable. Trust-minimised, not trustless.
Canton side: Daml contracts (planned) 1. Verify the certificate signature and integrity. 2. Record the verified measurement against a measurement-backed asset template. 3. Where a methodology is applied, compute derived quantities (e.g. CO2-equivalent, conservativeness deduction) as that methodology defines. The methodology is not yet confirmed and depends on the project's registry and verifier. 4. Expose the verified record to a registry, verifier or issuer, who keep issuance authority. KAILEdge does not issue carbon credits. 5. Support transfer and retirement workflows through template interfaces. The templates are intended to implement the Canton Network Token Standard (CIP-0056, https://docs.canton.network/overview/reference/cip-0056) interfaces where applicable.
Edge compute platform: Intel N305 (8C/8T, 15W TDP), 16 GB RAM, on-board TPM 2.0, fanless IP67-rated enclosure. Offline operation: certificates are queued locally during connectivity loss.
Hardware scope: three complete sets, 69 devices per set, 207 devices to be procured under T1. 69 to be installed live; 138 held as calibrated spares so measurement continues during calibration windows. Status today: specified, supplier quotations requested, nothing ordered.

### 3. Architectural Alignment
Canton RWA direction: this pilot would extend Canton's RWA work into measurement-backed environmental assets.
Related environmental asset work: Xpansiv announced a phased initiative to enable tokenization of environmental assets on Canton (https://www.xpansiv.com/xpansiv-to-enable-tokenized-environmental-asset-infrastructure-via-canton-network/). ClimateTrade has announced joining as a network validator. KAILEdge's measurement layer is intended to complement platforms like these. Neither has agreed to use it.
Token standard: templates intended to implement CIP-0056 interfaces where applicable.
Label: see header.
Open and proprietary: Apache 2.0 for Daml templates, interfaces, certificate schema, conformance tests and replay guide. The calibrated physics parameters are proprietary. A third party can verify a certificate's signature, integrity and chain without KAILEdge. Reproducing calibrated outputs requires KAILEdge.

### 4. Backward Compatibility
No backward compatibility impact. New deployment at a new facility.

## Milestones and Deliverables
Dates assume a signed host-site agreement with Sustainable Impacts; month 1 starts at signature.
T1 - Hardware deployment and first data: $60,000, Month 1-2. 207 devices procured (three complete sets); 69 installed at the Harohalli CBG facility; edge compute units deployed with TPM 2.0; Class A reference sensors calibrated by an NABL-accredited laboratory; first data transmitted from plant to edge node; first certificates generated and checked on Canton testnet.
T2 - Physics validation and certificate pipeline: $25,000, Month 2-3. Physics engine deployed on the edge node; equations calibrated against field data; methane mass uncertainty characterised (target <=8%); first TPM-sealed certificates verified by independent replay; calibration chain documented.
T3 - Canton integration: $20,000, Month 3-4. Daml contracts deployed on Canton testnet; certificate verification, measurement-backed asset, transfer and retirement templates; certificates consumed by smart contracts; workflow exercised end to end on testnet.
T4 - Reference case and acceptance: $15,000, Month 4-5. One full production cycle completed; verifier evidence package assembled; reference case published; replay guide and developer documentation delivered; open-source integration layer released.

## Possible follow-on uses (not funded by this grant, no commitments)
Spot and forward trading of verified carbon credits, tokenized CBG contracts, energy efficiency attestations, NPK-verified fertilizer records, and data tokens built on sealed measurement streams. These depend on registries, issuers and buyers. Nothing here assumes them.

## Acceptance Criteria
1. A reproducible, TPM-sealed certificate is generated from calibrated field sensors at the Harohalli CBG facility.
2. An independent reviewer can replay the certificate from the recorded inputs and model version and verify the hash.
3. The certificate is consumed by a Daml smart contract on Canton testnet.
4. Documentation and knowledge transfer are complete: reference case published, replay guide delivered, open-source integration layer released.

## Funding
Total: $120,000. T1 $60,000; T2 $25,000; T3 $20,000; T4 $15,000. Our separate Canton Foundation grant application is under evaluation and not secured.
Volatility stipulation: project duration is 5 months. The grant is denominated in Canton Coin at the amount the committee sets. The timeline does not extend beyond 5 months.

## Value to the Canton Ecosystem
1. A reference integration from device-sealed environmental measurements to Daml contracts, tested on testnet
2. A second vertical for the same measurement architecture (designed for cold chain, not yet built there)
3. Reusable open-source templates, schema, conformance tests and replay guide for later environmental assets
4. A route for CBG producers to supply verifiable evidence to registries, verifiers and buyers who use Canton
5. Any resulting Canton activity would generate network fees. No volume is assumed.

## Honest Position on Canton Adoption
No external Canton application currently consumes KAILEdge certificates. The pilot would create the first such integration and produce a reusable schema, Daml contract templates and a documented replay pathway. We are not claiming existing Canton adoption. We are in discussions with Sustainable Impacts about hosting the pilot. No agreement is signed.

## Risks and Mitigations
Host-site agreement not signed: T1 does not start until it is. Sensor calibration delays: an NABL-accredited laboratory will be engaged during procurement. Canton testnet integration complexity: Daml contracts developed in parallel with hardware deployment. Physics engine performance in field conditions: the pilot tests the architecture before any scale commitment, and results will be reported against the targets above. Regulatory compliance: a hazardous-area zoning assessment will be conducted before installation. Offline operation: certificates queued locally, backup power design to be confirmed.

## Contact
Prasad Gopal, Founder, KAILEdge, prasad@kailedge.com, www.kailedge.com
