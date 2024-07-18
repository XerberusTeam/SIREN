# ⚽ Planning & Project Kick-off
- High-level Architecture
- Tasks Projection
- Legal Analysis
- Blockchain Relationship Crawler Algorithm
- Link to SIREN Presentation Document
  </br>
</br>

## 📐 High-level Architecture
<img width="1415" alt="architecture" src="https://github.com/user-attachments/assets/5ba59fe2-9b47-4fed-a4bb-74a5c1b1af6b">
</br>
</br>

## 🔮 Tasks Projection
<img width="1409" alt="timeline" src="https://github.com/user-attachments/assets/20ba30d3-2f39-4482-a02f-ef31b65e6823">
</br>
</br>

## ⚖️ Legal Analysis

### 1. Object of the Analysis: SIREN
SIREN enables victims to report their public wallet address, the associated fraudulent incident, and the amount lost. Users can also identify suspected fraudsters' wallet addresses.
</br>
</br>
Xerberus Labs curates a list of these addresses, identified through a network graph algorithm, and publicly displays them to highlight bad actors in the crypto space. 
</br>
</br>
This initiative, while aimed at transparency and fraud prevention, raises significant data privacy and legal implications, especially concerning the potential reputational harm to individuals listed without definitive proof of wrongdoing.

### 2. Legal Challenges and Considerations
Analysis of potential legal concerns arising from the operation of the SIREN application by Xerberus Labs LTD, focusing on data protection and privacy laws.

#### Specific Legal Questions
- **Personal Data Classification:** Given that public wallet addresses (PWAs) can sometimes be
linked to individual identities, are they considered personal data under UK, EU, and US law?
- **Privacy Law Compliance:** What are the implications of publicly listing PWAs without the wallet owners' consent, especially in jurisdictions governed by strict privacy regulations like the GDPR?
- **Defamation Risk:** Evaluate the risk of defamation claims arising from publicly labeling individuals as fraudsters based on submitted reports and algorithmic determinations without legal proof.

### 3. Overview of Relevant Legal Frameworks
<img width="1104" alt="legal frameworks" src="https://github.com/user-attachments/assets/1498cf2e-faae-47f8-97b9-e625fb0b012b">

### 4. When Are Public Wallet Addresses Not Considered Personal Data?
#### Key Conditions
- **Isolation of Data:** PWAs are not connected
with or presented alongside any other identifying information that could link them to an individual.
- **Anonymity in Data Use:** The wallet addresses are used in such a way that they cannot be traced back to any individual, maintaining complete anonymity.
- **Lack of Identifiable Context:** The data (PWAs) is handled and displayed without context or additional data that could make it possible to identify the wallet owner.
#### Legal Perspective
Under UK and EU law, personal dat a is def ined as inf ormat ion t hat can directly or indirectly identify an individual. The UK General Data Protection Regulation (UK GDPR) and the EU General Data Protection Regulation (EU GDPR) both adhere to this definition, as articulated in Article 4(1) of the GDPR, which defines personal data as "any
inf ormat ion relat ing t o an ident if ied or ident if iable nat ural person ('data subject'); an identifiable natural person is on who can be identified, directly or indirectly, particularly by reference to an identifier such as a name, an identification number, location data, an online identifier or to one or more factors specific to the physical, physiological, genetic, mental, economic, cultural or social identity of that natural person."
</br>
</br>
Therefore, a public wallet address (PWA) is not considered personal data unless it can be linked to specific individuals through other data that makes them identifiable. This distinction is critical in applications like SIREN, where data anonymity can determine the legal obligations and potential liability under these regulations.

### 5. Assumption of the legal status of SIREN
- **Legal Risks:** There is a high risk under UK and EU laws if PWAs can be traced back to individuals,
considering both the UK GDPR and EU GDPR's strict guidelines on personal data.
- **Defamation Risks:** Potential defamation claims could arise if individuals are wrongly identified as fraudsters without sufficient evidence.

### 6. Solutions: Strategic Internal Recommendations
#### Potential Actions
- **Conduct a Data Protection Impact Assessment (DPIA):** This assessment will identify risks in data
processing and help mitigate them.
- **Implement Anonymization Techniques:** Use robust techniques to ensure that PWAs cannot be linked back to individuals, safeguarding against data protection breaches.
- **Transparency and Consent:** Clearly communicate with users about data processing practices and obtain explicit consent when necessary, especially where there is a risk that PWAs could be linked to individuals.
- **Regular Legal Review:** Continually assess and update data handling practices to stay compliant with evolving legal standards and technological developments.
</br>
</br>

## 🕸️ Blockchain Relationship Crawler Algorithm
Each wallet can have one of two relationships with other wallets: it can be a parent (received money or tokens from another wallet) or a child (sent money to another wallet). On-chain interactions often involve a few crucial wallets, known as 'Hubs', which include the minting wallet, decentralized exchanges (DEXs), centralized exchanges (CEXs), and protocol wallets.
</br>
</br>
The amount sent or received defines the weighted relationship between wallets. For a given wallet, we derive a subgraph of the overall "wallet graph" by identifying the wallet in question, its ancestors (parents of parents), and descendants (children of children). We perform a Breadth-First Search (BFS), assigning weights to each related wallet based on the percent dilution and the percent relation.
</br>
</br>
#### Percent Dilution
Percent dilution evaluates how much of wallet x's total interactions involve the wallet in question versus other wallets.
</br>
Formally, for a wallet x:
</br>
</br>
Percent Dilution = (Amount of interactions with the target wallet) / (Total amount of interactions)
</br>
</br>

#### Percent Relation
Percent relation is the product of dilutions through the BFS. If a wallet A is related to wallet B through a chain of interactions involving wallets C and D, the percent relation is:
</br>
</br>
Percent Relation_{A to B} = Dilution_{A to C} × Dilution_{C to D} × Dilution_{D to B}
</br>
</br>

#### Graph Construction
The algorithm, Siren, creates a subset graph G = (V, E) from the wallet graph WG = (V, E) for the wallet being analyzed. It returns the percent relation of each wallet to the wallet in question and the counted set of graphlets for G.
</br>
</br>

#### Detecting Malicious Activity
To identify malicious activities such as wash trading, the intersection of graphlets from the scammed parties' wallets can pinpoint specific fraudulent actions. For scam detection, the intersection of related wallets for each scammed party is used, excluding Hubs. This helps identify the related wallet, which is likely the scammer, based on its statistics. The money is then tracked from this wallet to its endpoint, typically a CEX, similar to the PageRank algorithm.
</br>
</br>

#### Mathematical Representation
1. Graph Representation:
- WG = (V, E): Wallet Graph
- G = (V, E): Subset Graph for the wallet in question

2. Percent Dilution:
- For a wallet x interacting with wallet y:
Percent Dilution_{x to y} = I_{x to y} / I_x
where I_{x to y} is the interaction amount between x and y, and I_x is the total interaction amount of x.

3. Percent Relation:
- For a chain of interactions A to C to D to B:
Percent Relation_{A to B} = Dilution_{A to C} × Dilution_{C to D} × Dilution_{D to B}

4. Graphlet Intersection:
- To detect wash trading or scams, intersect the graphlets of scammed wallets:
Graphlet Intersection = intersection_{i=1}^{n} Graphlet(G_i)

By applying these mathematical constructs, Siren effectively analyzes and detects relationships and potential fraud within blockchain transactions.
</br>
</br>

## 🔗 Links
[SIREN Presentation](https://docsend.com/view/5rnhmry5pxandkbh)
