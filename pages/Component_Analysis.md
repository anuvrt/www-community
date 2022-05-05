---

title: Component Analysis
layout: col-sidebar
author: Steve Springett
contributors:
tags: csrm, sca, scca, oss, sbom
permalink: /Component_Analysis

---

{% include writers.html %}

Modern software is assembled using third-party and open source components, glued together in complex and unique ways, 
and integrated with original code to provide the desired functionality. Third-party (including commercially licensed,
proprietary, and "source available" software) along with open source components provide the necessary building blocks 
that allow organizations to deliver value, improve quality, reduce risk and time-to-market. The benefits of open source 
are many. However, by using open source components, organizations ultimately take responsibility for code they did not 
write. Strategic alliances between organizations and open source projects can lead to healthy open source usage and 
overall risk reduction.

Component Analysis is the process of identifying potential areas of risk from the use of third-party and open-source 
software and hardware components. Component Analysis is a function within an overall [Cyber Supply Chain Risk
Management](https://csrc.nist.gov/projects/supply-chain-risk-management) (C-SCRM) framework. A software-only subset of 
Component Analysis with limited scope is commonly referred to as Software Composition Analysis (SCA).

Any component that has the potential to adversely impact cyber supply-chain risk is a candidate for Component Analysis.

## Common Risk Factors

#### Component Inventory

Having an accurate inventory of all third-party and open source components is pivotal for risk identification. Without 
such knowledge, other factors of Component Analysis become impractical to determine with high confidence. Use of 
dependency management solutions and/or [Software Bill of Materials (SBOM)](#software-bill-of-materials-sbom) can aid in
inventory creation.

#### Component Age

Components may have varying degrees of age acceptance criteria. Factors that impact acceptable age include the [type of
component](#component-type), the ecosystem the component is a part of (Maven, NPM, etc), and the purpose of the 
component. The age of a component may signify use of outdated technology and may have a higher probability of being 
passed over by security researchers.

#### Outdated Components

Newer versions of components may improve quality or performance. These improvements can be inherited by the applications 
that have a dependency on them. Components that are end-of-life (EOL) or end-of-support (EOS) also impact risk. Two 
common approaches to community supported open-source is:

  - Support the latest revision of the last (x) releases - (i.e. 4.3.6 and 4.2.9)
  - Support only the latest published version (i.e. 4.3.6 today and 4.4.0 tomorrow)

Depending on the risk appetite, it may be strategic to only use third-party and open source components that are supported.

With reference to [Semantic Versioning](https://semver.org/) terminology, API changes can be expected between major 
versions of a component, but are rare between minor versions and patches. Changes in a components API may result in 
longer remediation times if the components have not been continuously updated. Keeping components up-to-date can
reduce remediation time when a rapid response is warranted.

Outdated version identification may leverage ecosystem-specific repositories and is achievable through the use of 
dependency managers or [Package URL](#component-identification). Component analysis can identify outdated components as 
well as provide information about newer versions.

#### Known Vulnerabilities

Historically, known vulnerabilities referred to entries (CVEs) in the 
[National Vulnerability Database](https://nvd.nist.gov/) (NVD). The NVD describes (via [CPE](https://nvd.nist.gov/products/cpe)) 
three types of components:

  - Applications (includes libraries and frameworks)
  - Operating Systems
  - Hardware

While the NVD may be the most recognizable source of vulnerability intelligence, it's not the only. There are multiple 
public and commercial sources of vulnerability intelligence. Having a *known* vulnerability doesn't require the 
vulnerability information be present in one of these sources. Simply being documented (i.e. social media post, defect 
tracker, commit log, release notes, etc) classifies the vulnerability as being *known*.

Component analysis will commonly identify known vulnerabilities from multiple sources of vulnerability intelligence.

#### Component Type

Frameworks and libraries have unique upgrade challenges and associated risk. Abstractions, coupling, and architectural 
design patterns may affect the risk of using a given component type. For example, logging libraries may be embedded 
throughout a large application, but replacing implementations can likely be automated. Likewise, replacing a web 
application framework for an alternative framework would likely be a high-risk endeavor leading to architectural changes, 
regressions, and code rewrites. Evaluating the type should be part of every Component Analysis strategy.

#### Component Function

Identifying and analyzing the purpose of each component may reveal the existence of components with duplicate or similar 
functionality. For example, it's unlikely an application would need multiple XML parsers or cryptographic providers. 
Potential risk can be reduced by minimizing the number of components for each function and by choosing the highest
quality components for each function. Evaluating component function should be part of every Component Analysis strategy.

#### Component Quantity

The number of third-party and open source components in a project should be evaluated. The operational and maintenance 
cost of using open source will increase with the adoption of every new component. The impact can be significantly higher 
with micro-module ecosystems when there are hundreds or thousands of applications in a given environment. In addition to 
increased operational and maintenance cost, a decrease in a development teams ability to maintain growing sets of 
components over time can be expected. This is especially true for teams with time-boxed constraints.

#### Repository Trust

Components in many software ecosystems are published and distributed to central repositories. Repositories have known 
threats. Some of the threats against public repositories include:

  - Typosquatting - naming a component in such as way as to take advantage of common misspelling
  - Organization/Group abuse - pretending to be a public person or entity and abusing the perceived trust. This includes
    dependency confusion and namespace confusion.
  - Malware through transfer - leveraging weak or absent code-signing requirements to spread malware through the 
    transfer of an open source project from one maintainer to another
  - Cross Build Injection (XBI) - Abusing dependency resolution schemes and weak infrastructure controls to inject 
    malicious components in place of safe ones

Public repositories that have code-signing and verification requirements have some level of trust, whereas public 
repositories without basic countermeasures do not. For no-trust or low-trust repositories, utilizing private 
repositories may be advantageous. Private repositories refer to repositories where access is limited, usually software 
that organizations install and control, or a commercially available service. *Golden repositories* containing vetted or 
permitted components are a common use-case for private repositories. Private repository services focusing on security 
may additionally provide anti-malware analysis and static source code analysis requirements prior to acceptance in the
repository. When leveraging private repositories, it is important to have traceability to the components' provenance.

#### Provenance

A component's provenance refers to the traceability of all authorship, build, release, packaging, and distribution 
across the entire supply chain. In physical supply chains this is referred to as the *chain of custody*. Provenance may 
include individual and community authorship of software components, manufacturers, suppliers, software repositories, 
and country of origin. For high assurance applications, provenance plays an important role in determining Foreign 
Ownership, Control, or Influence (FOCI).

#### Pedigree

Data which describes the lineage for which software has been created or altered. Pedigree includes the ancestors, 
descendants, and variants which describe component lineage from any viewpoint and the commits, patches, and diffs which 
make a component unique. Maintaining accurate pedigree information is especially important with open source components whos 
source code is readily available, modifiable, and redistributable. 

#### Formulation

Formulation describes how components were built often including build system invocation and properties, SDK and compiler
versions, compiler flags, and a comprehensive list of parallel and sequential steps that were taken to build, test, and 
deliver a component. Formulation and pedigree are complimentary concepts and are often combined and referred simply
as pedigree.

#### License

Third-party and open-source software typically has one or more licenses assigned. The chosen license may or may not 
allow certain types of usage, contain distribution requirements or limitations, or require specific actions if the 
component is modified. Component Analysis can identify the license(s) for a given component and may optionally provide
guidance as to the nature of the license (i.e. copyright, copyleft, OSI approved, etc). Utilizing components with 
licenses which conflict with an organizations objectives or ability can create serious risk to the business.

#### Inherited Risk

Third-party and open source components often have dependencies on other components. A *transitive dependency* is when 
an application has a direct dependency on a component and that component has a dependency on another component. 
Transitive dependencies are common and are expected in highly modular ecosystems which values reuse over re-invent. 
Like any component, transitive dependencies have their own risk which is inherited by every component and application 
that relies on them. Components may additionally have specific runtime or environmental dependencies with implementation 
details not known or prescribed by the component. Component Analysis can aggregate the risk of all direct, transitive, 
runtime, and environmental dependencies providing a holistic view of inherited risk.

#### Project Health

There are many datapoints to consider when evaluating the health of an open source project.

  - Quality Controls and Metrics - The overall quality and controls for achieving and maintaining high-quality 
    components may be a factor in risk evaluation. For software components, this refers to the use of unit and 
    integration tests, linters and static analysis tools, the percentage of coverage, and results from various tools.
  - Community Engagement - The current and historical trend for a project and its maintainers to accept pull requests, 
    answer defect and enhancement requests, and engage in productive collaboration with the community may be a factor 
    in risk evaluation.
  - Vulnerability Analysis - Analyzing current and historical security vulnerabilities for timeline trends and for 
    root-cause patterns may signify a projects ability to protect the community from future (and similar) issues. 
    This activity may be a factor in risk evaluation.

#### External Services

Applications or their direct or transitive components may rely on external services for functionality. Common examples 
include convenience libraries around common web services such as mapping, stock quotes, or weather, but may also include
autogenerated software components built from API specifications such as TUF or OpenAPI. External services should be 
included in the overall inventory of components.

## Software Bill of Materials (SBOM)

Multiple efforts between government and industry are attempting to define *Software Transparency*. Some of these efforts 
will lead to increased compliance or regulatory requirements. Software Transparency is often achieved through the 
publishing of software bill of materials. An SBOM is synonymous to the list of ingredients in a recipe. Both are an
implementation of transparency.

There are multiple SBOM standards including OWASP [CycloneDX](https://cyclonedx.org/) and [SPDX](https://spdx.org/), each 
having their own strengths and use-cases they were designed to solve. Evaluating SBOM standards to determine which are 
applicable to an organizations requirements should be part of an overall C-SCRM strategy.

## Component Identification

Component ecosystems generally devise different terminology and formats for representing components. This self-imposed 
fragmentation makes uniquely identifying and representing components difficult when referring to them outside of their 
respective ecosystems. Centralized databases such as the [CPE Product Dictionary](https://nvd.nist.gov/products/cpe) 
maintained by [NIST](https://www.nist.gov/) adds additional fragmentation as the CPE vendor and product names often do 
not reflect reality.

[SWID](https://www.iso.org/standard/65666.html) tags are an alternative approach to component identity. They have 
historically been used to describe commercial software for the purpose of license entitlements and ITOM discovery within
a CMDB system. Software manufactures can create their own SWID tags whenever new versions are released.

Generally, a component will have a name and version. Components may optionally have a higher-level grouping identifier, 
commonly referred to as a groupId, organization, or vendor. When referencing components in a C-SCRM framework it is 
important to have a standard and uniform way to represent them. The [Package URL](https://github.com/package-url)
[specification](https://github.com/package-url/purl-spec) provides a decentralized and uniform way to represent components.

```
    scheme:type/namespace/name@version?qualifiers#subpath
```

For example:
```
    pkg:deb/debian/curl@7.50.3-1?arch=i386&distro=jessie
    pkg:docker/cassandra@sha256:244fd47e07d1004f0aed9c
    pkg:gem/jruby-launcher@1.1.2?platform=java
    pkg:maven/org.apache.xmlgraphics/batik-anim@1.9.1?packaging=sources
    pkg:npm/%40angular/animation@12.3.1
    pkg:nuget/EnterpriseLibrary.Common@6.0.1304
    pkg:pypi/django@1.11.1
    pkg:rpm/fedora/curl@7.50.3-1.fc25?arch=i386&distro=fedora-25
```

## Software Verification

Multiple standards exist for the verification of software components including: 

* [OWASP Software Component Verification Standard (SCVS)](http://owasp.org/scvs) 
* [Supply-chain Levels for Software Artifacts (SLSA)](https://slsa.dev/)

Both standards aim to measure and improve the assurance of the software supply chain.

## Open Source Policy

Open source policies provide guidance and governance to organizations looking to reduce third-party and open source 
risk. Policies typically include:

  - Restrictions on component age
  - Restrictions on outdated and EOL/EOS components
  - Prohibition of components with known vulnerabilities
  - Restrictions on public repository usage
  - Restrictions on acceptable licenses
  - Component update requirements
  - Deny list of prohibited components and versions
  - Acceptable community contribution guidelines

While the open source policy is usually filled with restrictions, it provides an organizations security, development,
and legal teams an opportunity to create solutions for healthy open source usage.

## Recommendations

  - Limit the age of acceptable components to three years or less with exceptions being made for high-value, single 
    purpose components that are still relevant
  - Prohibit the use of end-of-life (EOL) components
  - Prohibit the use of components with known vulnerabilities. Update components that are exploitable with high to 
    moderate risk first.
  - Reduce the attack surface by excluding unnecessary direct and transitive dependencies
  - Reduce the number of suppliers and use the highest quality components from those suppliers
  - Standardize on a single component for each component function
  - Establish a maximum level of acceptable risk for public repositories. Utilize private repositories in lieu of 
    untrusted ones.
  - Automate component updates (from trusted repositories only)
  - Provide time-boxed allowances every sprint to maintain component hygiene
  - Establish a allowed list of acceptable licenses, a deny list of prohibited licenses, and seek advice from counsel for 
    all other licenses
  - Automate the creation of software bill-of-materials (SBOM) for all deliverables
  - Leverage Package URL for describing components within SBOMs
  - Contractually require SBOMs from vendors and embed their acquisition in the procurement process
  - Automate the analysis of all third-party and open source components during Continuous Integration (CI), either by 
    analyzing the files themselves, or by analyzing a SBOM
  - Import SBOMs into systems capable of tracking, analyzing, and proactively monitoring all components used by every 
    asset in an environment (i.e. enterprise wide, entire cloud infrastructure, etc)

## Tools Listing

| Name | Tool Summary | Owner | Licence | Platforms |
| ---- | ----- | ----- | ------- |---------- |
| [GitHub SCA] | GitHub SCA analyze open source components; Identifies, priortize based on set policies and provide best possible upgrade recommendations to developers | GitHub | Freemium | SaaS |
| [Black Duck Hub] | Black Duck® software composition analysis (SCA) helps teams manage the security, quality, and license compliance risks that come from the use of open source and third-party code in applications and containers. | Synopsys | Commercial | Cross Platform |
| [Bytesafe] | Scan and identify open source risk directly in your applications, registries and repositories | Bytesafe | Freemium | SaaS |
| [Bytesafe Dependency Checker] | Check dependencies and get the results for your project directly in your browser | Bytesafe | Free | SaaS |
| [CAST Highlight] | Pinpoint the most critical open source risks; Detect Common Vulnerabilities & Exposures; Detect Common Weakness Enumerations; Detect all licenses in use across components at the portfolio and application levels to identify possible legal issues. Customize the license profile policy to meet specific needs of the organization ; Instantly detect which applications use obsolete component versions that require upgrades and get recommendations on safer versions to use; Detect open source vulnerability and license risks buried in dependent components that your open source components use; Analyze complex applications that use numerous components more easily using data visualization. | CAST Software | Commercial | SaaS |
| [Clarity] | Insignary Clarity leverages unique fingerprint matching technology that extracts “fingerprints” from the target binary code to examine and then compare against the fingerprints collected from open source components in numerous open source repositories. | Insignary | Commercial | Cross Platform / SaaS |
| [ClearlyDefined] | ClearlyDefined is focused on clarifying data about open source components. Specifically, the initial focus is on three key pieces of data about open source: license, source location, and attribution parties. Clarity on these pieces of data helps everyone know what their obligations are and feel more confident in meeting them. | Open Source Initiative | Open Source | SaaS |
| [CodeSentry] | CodeSentry is derived from GrammaTech’s ground-breaking binary code analysis research. This technology achieves deep scalable analysis without the need for source code and is suitable for enterprise wide adoption.  Binary analysis is both efficient and less error prone than conventional SCA tools and due to CodeSentry’s high precision and recall results in fewer missed vulnerabilities and fewer false positives. The key advantage of CodeSentry is the ability to interrogate – at the binary level - both open source software and the third-party software that is now so commonly used. | GrammaTech | Commercial | Cross Platform / SaaS |
| [CxSCA] | Checkmarx SCA™ allows your developers to build software with confidence using a mix of custom and open source code. You need to know the libraries they’re using are secure. Checkmarx SCA is the software composition analysis tool designed to do exactly that, backed by an expert research team uncovering the latest open source risks. | Checkmarx | Commercial | SaaS |
| [Debricked] | Continuously and automatically identify, fix and prevent vulnerabilities in open source dependencies. Scan at every commit and get notified when new vulnerabilities appear; Ensure and maintain open source compliance with automated and enforceable pipeline rules. Calculate risk levels for your repositories based on intended use ; Evaluate and compare open source dependencies before intake to ensure high quality. Monitor projects over time, get community insights and identify potential risks. | Debricked | Commercial/Freemium | SaaS |
| [DejaCode] | Identify any open source components and their license compliance data in an application codebase; Generate an inventory of components and their licenses to use as the baseline for your FOSS compliance process ;  100% open source under Apache 2.0 and other business-friendly licenses with support for all programming languages and environments | nexB | Commercial | SaaS |
| [Dependency-Check] | Dependency-Check is a Software Composition Analysis (SCA) tool that attempts to detect publicly disclosed vulnerabilities contained within a project’s dependencies. It does this by determining if there is a Common Platform Enumeration (CPE) identifier for a given dependency. If found, it will generate a report linking to the associated CVE entries. | OWASP | Open Source | Cross Platform |
| [Dependency-Track] | Dependency-Track is an intelligent Component Analysis platform that allows organizations to identify and reduce risk in the software supply chain. Dependency-Track takes a unique and highly beneficial approach by leveraging the capabilities of Software Bill of Materials (SBOM). This approach provides capabilities that traditional Software Composition Analysis (SCA) solutions cannot achieve. | OWASP | Open Source | Cross Platform |
| [Dependabot] | Dependabot checks for outdated dependencies as soon as it's enabled. You may see new pull requests for version updates within minutes of adding the configuration file, depending on the number of manifest files for which you configure updates. | Dependabot | Commercial / Freemium | SaaS |
| [DepShield] | Sonatype DepShield is a GitHub App used by developers to identify and remediate vulnerabilities in their open source dependencies. | Sonatype | Open Source | Cross Platform / SaaS |
| [DotNET Retire] | <> | Retire.NET Project | Open Source | Cross Platform |
| [FlexNet Code Insight] | FlexNet Code Insight is a single integrated solution for open source license compliance and security. Find vulnerabilities and remediate associated risk while you build your products and during their  entire lifecycle. Manage open source license compliance, add automation to your processes, and implement a formal OSS strategy that balances business benefits and risk management. | Flexera Commercial	| Cross Platform |
| [Fluid Attack's Scanner] | Fluid Attacks' scanner is a security vulnerability detection tool. This means it scans your source-code, infrastructure and applications and reports to you the security problems they have. | Fluid Attacks | MPL 2.0. | SaaS |
| [FOSSA] | Open Source License Compliance Management | FOSSA | Commercial / Freemium | SaaS |
| [FOSSology] | FOSSology is a open source license compliance software system and toolkit. As a toolkit you can run license, copyright and export control scans from the command line. As a system, a database and web ui are provided to give you a compliance workflow. License, copyright and export scanners are tools available to help with your compliance activities. | Linux Foundation | Open Source | Cross Platform |
| [Grafeas] | Defines a uniform way for auditing and governing the modern software supply chain; Grafeas provides organizations with a central source of truth for tracking and enforcing policies across an ever growing set of software development teams and pipelines. Build, auditing and compliance tools can use the Grafeas API to store, query and retrieve comprehensive metadata on software components of all kinds. | Grafeas | Open Source | Cross Platform |
| [Greenkeeper] | Tool Not available | Greenkeeper | Open Source | SaaS |
| [Ion Channel SA] | While SCA tools and scanners provide snapshots of known vulnerabilities, Ion Channel maintains time-based records of how long known vulnerabilities are allowed to persist in open source components and supplier products, as well as leading risk indicators like end-of-life, maintenance risk, technical debt and ecosystem risk. | Ion Channel | Commercial | SaaS |
| [Libraries.io] | Search 5.45M packages by license, language or keyword, or explore new, trending or popular packages ; Stay up to date with notifications of updates, license incompatibilities or deleted dependencies ; Maintain your OSS project; Use Libraries.io data in your applications, services or research. Use APIs to stay up to date. | Tidelift | Open Source | SaaS |
| [MergeBase] | MergeBase’s SCA platform manages vulnerabilities and license risk during coding, building, deployment and running of your applications. It provides developer guidance based on risk, compatibility and popularity. It triggers warnings about vulnerabilities applications running in production including from third-party components and third-party software. | MergeBase | Commercial | SaaS |
| [Nexus IQ] | Automatically detect and fix open source dependency vulnerabilities; Integrate security vulnerability tools into git repositories you already use; View your components, their vulnerabilities, licenses, and dependencies all in one place for easier monitoring ; Avoid attacks through scaled secure development practices across dev and ops teams | Sonatype | Commercial | Cross Platform |
| [NPM Audit] | | NPM | Open Source | SaaS |
| [Open Source Lifecycle Management] | View your components, their vulnerabilities, licenses, and dependencies all in one place for easier monitoring. | WhiteSource Software | Commercial | Cross Platform / SaaS |
| [OSS Index] | OSS Index is a free catalogue of open source components and scanning tools to help developers identify vulnerabilities, understand risk, and keep their software safe. | Sonatype | Free / Open Source | SaaS |
| [OSS Review Toolkit] | The OSS Review Toolkit (ORT) aims to assist with the tasks that commonly need to be performed in the context of license compliance checks, especially for (but not limited to) Free and Open Source Software dependencies. It does so by orchestrating a highly customizable pipeline of tools that abstract away the underlying services.  | HERE | Open Source | Cross Platform |
| [Patton] | Patton is a set of tools for helping admins and security auditors to search for vulnerabilities in software components and computer systems. Patton contains, at this moment, the tools needed to analyze vulnerabilities on Ubuntu, Debian and RHEL 8 or newer systems and we are currently working on increasing the targets to include Python dependencies. | OWASP | Open Source | Cross Platform |
| [PHP Security Checker] | Tool is not functional now | SensioLabs | Open Source | SaaS |
| [Prisma Cloud] | Cloud Security Posture Management, Cloud workload protection, Cloud network security, cloud identity security | Palo Alto Networks | Commercial | Cross Platform / SaaS |
| [Renovate] | Continuously runs and detects the latest available versions | Key Location | Open Source | SaaS |
| [Retire.js] | The goal of Retire.js is to help you detect use of version with known vulnerabilities. Retire.js has these parts: 1- A command line scanner, 2- A grunt plugin, 3-A Chrome extension, 4- A Firefox extension, 5- Burp and OWASP Zap plugin | RetireJS Project | Open Source | Cross Platform |
| [Snyk] | Detect vulnerabilities in code, open source, containers, infrastructure as code | Snyk | Commercial / Freemium | SaaS |
| [SourceClear] | Detect open source vulnerabilities | Veracode | Commercial | Cross Platform / SaaS |
| [VulnDB] | VulnDB allows organizations to search and be alerted on the latest vulnerabilities, both in end-user software and the 3rd Party Libraries or dependencies. A subscription to VulnDB provides organizations with simple to understand ratings and metrics on their vendors and products, and how each contributes to the organization’s risk-profile and cost of ownership. | Risk Based Security | Commercial	| SaaS |
| [Vigiles] | Vulnerability monitoring and remediation tool that combines a curated CVE database, continuous security feed based on your SBOM,  filtering, and easy triage tools | Timesys | Commercial / Freemium | SaaS |
| [Vigilant Ops InSight] | Continous vulnerability monitoring ; Vigilant Ops Insight provides a cloud-based platform for creating, maintaining, and securely sharing Cybersecurity Bill of Materials (CBOM)  | Vigilant Ops | Commercial | Cross Platform / SaaS |
| [Xray] | JFrog Xray is an application security SCA tool that integrates security directly into your DevOps workflows, enabling you to deliver trusted software releases faster. JFrog Xray fortifies your software supply chain and scans your entire pipeline from your IDE, through your CI/CD Tools, and all the way through distribution to deployment. | JFrog | Commercial | Cross Platform |

[Bytesafe]: https://bytesafe.dev/
[Bytesafe Dependency Checker]: https://bytesafe.dev/dependency-checker/javascript/
[GitHub SCA]: https://docs.github.com/en/github/visualizing-repository-data-with-graphs/listing-the-packages-that-a-repository-depends-on/
[Black Duck Hub]: https://www.blackducksoftware.com/
[CAST Highlight]: https://www.castsoftware.com/SCA/
[Clarity]: https://www.insignary.com/
[ClearlyDefined]: https://clearlydefined.io/
[CodeSentry]: https://www.grammatech.com/codesentry-sca
[CxSCA]: https://www.checkmarx.com/products/software-composition-analysis/
[Debricked]: https://debricked.com/
[DejaCode]: https://www.nexb.com/
[Dependency-Check]: https://owasp.org/www-project-dependency-check/
[Dependency-Track]: https://owasp.org/www-project-dependency-track/
[Dependabot]: https://dependabot.com/
[DepShield]: https://depshield.github.io/
[DotNET Retire]: https://github.com/RetireNet/dotnet-retire
[FlexNet Code Insight]: https://www.flexera.com/products/software-composition-analysis/flexnet-code-insight.html
[Fluid Attack's Scanner]: https://docs.fluidattacks.com/machine/scanner
[FOSSA]: https://fossa.com/
[FOSSology]: https://www.fossology.org/
[Grafeas]: https://grafeas.io/
[Greenkeeper]: https://greenkeeper.io/
[OSS Review Toolkit]: https://github.com/heremaps/oss-review-toolkit
[Ion Channel SA]: https://ionchannel.io/
[Libraries.io]: https://libraries.io/
[MergeBase]: http://mergebase.com/
[NPM Audit]: https://www.npmjs.com/
[OSS Index]: https://ossindex.sonatype.org/
[PHP Security Checker]: https://github.com/sensiolabs/security-checker
[Prisma Cloud]: https://www.paloaltonetworks.com/prisma/cloud
[Renovate]: https://renovatebot.com/
[Snyk]: https://snyk.io/
[SourceClear]: https://www.sourceclear.com/
[Nexus IQ]: https://www.sonatype.com/
[Open Source Lifecycle Management]: https://www.whitesourcesoftware.com/
[Retire.js]: https://retirejs.github.io/retire.js/
[VulnDB]: https://vulndb.cyberriskanalytics.com/
[Patton]: https://owasp.org/www-project-patton/
[Vigiles]: https://www.timesys.com/security/vigiles-vulnerability-management-patch-monitoring/
[Vigilant Ops InSight]: https://vigilant-ops.com/
[Xray]: https://jfrog.com/xray/


## References

  - Cyber Supply Chain Risk Management (C-SCRM)
    - <https://csrc.nist.gov/projects/supply-chain-risk-management>
  - Impact of software supply chain practices: Development Waste (Sonatype)  
    - <http://stwww-production.herokuapp.com/calculator/>
  - Content of Premarket Submissions for Management of Cybersecurity in Medical Devices (FDA)  
    - <https://www.fda.gov/downloads/MedicalDevices/DeviceRegulationandGuidance/GuidanceDocuments/UCM623529.pdf>
  - CycloneDX specification
    - <https://cyclonedx.org/>
  - Software Package Data Exchange (SPDX)  
    - <https://spdx.org/>
  - SWID (ISO/IEC 19770-2:2015)  
    - <https://www.iso.org/standard/65666.html>
  - Package URL specification and reference implementations  
    - <https://github.com/package-url> 
  - Using Software Bill-of-Materials to drive change and reduce risk  
    - <https://medium.com/@steve_springett/using-software-bill-of-materials-to-drive-change-and-reduce-risk-5901b7a339e3>
  - The Unfortunate Reality of Insecure Libraries (Contrast Security)
    - <https://cdn2.hubspot.net/hub/203759/file-1100864196-pdf/docs/Contrast_-_Insecure_Libraries_2014.pdf>
  - Framework for Software Supply Chain Integrity (SAFECode)  
    - <https://safecode.org/wp-content/uploads/2014/06/SAFECode_Supply_Chain0709.pdf>
  - Managing Security Risks Inherent in the Use of Third-party Components (SAFECode)  
    - <https://safecode.org/wp-content/uploads/2017/05/SAFECode_TPC_Whitepaper.pdf>
  - Deliver Uncompromised (MITRE)  
    - <https://www.mitre.org/sites/default/files/publications/pr-18-2417-deliver-uncompromised-MITRE-study-26AUG2019.pdf>
