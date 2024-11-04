# 22. Integration quality scale

Date: yyyy-mm-dd

## Status

Accepted

## Context

Home Assistant is a powerful open-source home automation system that enables you and all other users to automate and control various smart devices and services in their homes. We have established the integration quality scale to ensure a high-quality and consistent user and contributor experience for our open-source project.

This scale provides a framework for everybody contributing to enhancing the integrations while offering transparency regarding the reliability and capabilities of integrations offered to our users.

Here is an overview of the different tiers in the quality integration scale:

🥉 Bronze: This is the baseline standard and requirement for all new integrations added to Home Assistant. The integration can be configured via the UI, and there is documentation on how to set it up.

🥈 Silver: Enhanced reliability and robustness of the integration, ensuring a solid runtime experience. The integration handles errors correctly, including authentication errors and offline devices. The documentation provides more information on what the integration provides. The integration has one or more code owners who help to maintain the integration and ensure this runtime experience lasts.

🥇 Gold: The gold standard in delivering the best possible user experience with advanced features and comprehensive support. The integration is fully featured, user-friendly, and accessible to a wider audience. It is fully translated, capable of discovering its devices, providing device updates through Home Assistant, and has extensive documentation with examples, use cases, and troubleshooting. The integration provides means for debugging issues and can be fully adjusted through the UI.

🏆 Platinum: The epitome of quality within Home Assistant. Not only does this integration provide an outstanding user experience, but it also adheres to the highest technical standards with optimized performance, efficiency, and code quality.

Besides the four scaled tiers above, we also have four additional special tiers:

❓ No score: An integration that is available via the UI but has not been evaluated against the integration quality scale or does not meet the minimum requirements for scoring.

🏠 Internal: This is an integration used internally by Home Assistant to provide basic components and building blocks for the core program of Home Assistant or for other integrations to build on. The Home Assistant project maintains integrations of this level.

💾 Legacy: An older integration that has not yet transitioned to the Home Assistant user interface and lacks modern (nowadays considered normal) features.

📦 Custom: The Home Assistant project does not create, maintain, audit, review, or support custom integrations; therefore, they cannot be graded and placed on the integration quality scale.

## Decision

### Scaled tiers

Home Assistant integration quality scale has four differentiated tiers to which an integration can be graded: 🥉 Bronze, 🥈 Silver, 🥇 Gold, and 🏆 Platinum.

Each tier builds on the previous tier; for example, “Silver” has everything required by “Bronze”, and “Gold” has everything both “Silver” and “Bronze” require.

While some integrations may not be graded in the top tier, they might still implement parts of other (higher grade) tiers.

#### 🥉 Bronze

The bronze tier has the following characteristics:
- Can be easily set up through the UI.
- The source code adheres to basic coding standards and development guidelines.
- Automated tests that guard this integration can be configured correctly.
- Offers basic end-user documentation that is enough to get users started step-by-step easily.

#### 🥈 Silver

The silver tier has the following characteristics:
- Provides everything “Bronze” has.
- Provides a stable user experience under various conditions.
- Has one or more active code owners who help maintain the integration.
- Correctly and automatically recover from connection errors or offline devices, without filling log files and without unnecessary messages.
- Automatically triggers re-authentication if authentication with the device or service fails.
- Offers detailed documentation of what the integration provides and instructions for troubleshooting issues.

#### 🥇 Gold

The gold tier has the following characteristics:
- Provides everything “Silver” has.
- Has the best end-user experience an integration can offer; streamlined and intuitive.
- Can be automatically discovered, simplifying the integration setup.
- Integration can be reconfigured and adjusted.
- Supports translations.
- Extensive documentation, aimed at non-technical users.
- It supports updating the software/firmware of devices through Home Assistant when possible.
- The integration has automated tests covering the entire integration.
- Required level for integrations providing devices in the Works with Home Assistant program.

#### 🏆 Platinum

The platinum tier has the following characteristics:
- Provides everything “Gold” has.
- All source code follows all coding and Home Assistant integration standards and best practices and is fully typed with type annotations and clear code comments for better code clarity and maintenance.
- A fully asynchronous integration code base ensures efficient operation.
- Implements efficient data handling, reducing network and CPU usage.

### Special tiers

Besides the four scaled tiers mentioned above, Home Assistant also provides four special tiers to denote a special case, causing them not to fit into the regular scale set out above.

Note that while an integration might have been given one of these special tiers, they still might pursue the integration quality equivalent to any of the other scaled tiers.

#### ❓ No score
These integrations can be set up through the Home Assistant user interface. The “No score” designation doesn’t imply that they are bad or buggy, instead, it indicates that they haven’t been assessed according to the quality scale or that they need some maintenance to reach the now-considered minimum “Bronze” standard.

The “No score” tier cannot be assigned to new integrations, as they are required to have at least a “Bronze” level when introduced. The Home Assistant project encourages the community to help update these integrations without a score to meet at least the “Bronze” level requirements.

Characteristics:
- Not yet scored or lacks sufficient information for scoring.
- Can be set up via the UI, but may need enhancements for a better experience.
- May function correctly, but hasn’t been verified against current standards.
- Documentation most often provides only basic setup steps.

#### 🏠 Internal
This tier is assigned to integrations used internally by Home Assistant. These integrations provide basic components and building blocks for Home Assistant's core program or for other integrations to build on top of it.

Everything in Home Assistant is extendable using integrations. In fact, Home Assistant uses its own integration logic to extend base functionality. For example, the automation engine of Home Assistant is an integration. Those internal parts are thus marked with this “Internal” tier.

Internal integrations are maintained by the Home Assistant project and subjected to strict architectural design procedures.

Characteristics:
- Internal, built-in building blocks of the Home Assistant core program.
- Provides building blocks for other integrations to use and build on top of.
- Maintained by the Home Assistant project.

#### 💾 Legacy
Legacy integrations are older integrations that have been part of Home Assistant for many years, possibly since its inception. They can only be configured through YAML files and often lack active maintainers (code owners). These integrations might be complex to set up and do not adhere to current/modern end-user expectations in their use and features.

The Home Assistant project encourages the community to help migrate these integrations to the UI and update them to meet modern standards, making these integrations accessible to everyone.

Characteristics:
- Complex setup process; only configurable via YAML, without UI-based setup.
- May lack active code ownership and maintenance.
- Could be missing recent updates or bug fixes.
- Documentation may still be aimed at developers.

#### 📦 Custom
Custom integrations are developed and distributed by the community, and offer additional functionalities and support for devices and services to Home Assistant. These integrations are not included in the official Home Assistant releases and can be installed manually or via third-party tools like HACS (Home Assistant Community Store).

The Home Assistant project does not review, security audit, maintain, or support third-party custom integrations. Users are encouraged to exercise caution and review the custom integration’s source and community feedback before installation.

Developers are encouraged and invited to contribute their custom integration to the Home Assistant project by aligning them with the integration quality scale and submitting them for inclusion.

Characteristics:
- Not included in the official Home Assistant releases.
- Manually installable or installable via community tools, like HACS.
- Maintained by individual developers or community members.
- User experience may vary widely.
- Functionality, security, and stability can vary widely.
- Documentation may be limited.

### Adjusting the tier of an integration
Home Assistant encourages our contributors to get their integrations to the highest possible tier, to provide an excellent coding experience for our contributors and the best experience for our users.

When an integration reaches the minimum requirements for a certain tier, a contributor can open a pull request to adjust the scale for the integration. This request needs to be accompanied by the full checklist for each rule of scale (including all rules of lower tiers), demonstrating that it has met those requirements.

Once the Home Assistant core team reviews and approves it, the integration will display the new tier as of the next major release of Home Assistant.

Besides upgrading an integration to a higher tier on the scale, it is also possible for an integration to be downgraded to a lower tier. This can, for example, happen when there is no longer an active integration code owner. In this specific example, the integration will be downgraded to “Bronze,” even if it otherwise fully complies with the “Platinum” tier.

### Adjustments to rules contained in each tier
The world of IoT and all technologies used by Home Assistant are changing at a fast pace. Not just in terms of what Home Assistant can support or do, but also the software Home Assistant is built upon is constantly innovated. Home Assistant is pioneering the technology in the industry at a fast pace.

This also means that new insights and newly developed and adopted best practices will occur over time, resulting in new additions and improvements to the individual integration quality scale rules.

If a tier is adjusted, all integrations in that tier need to be re-evaluated and adjusted accordingly. One exception to this is integrations that have devices that are part of the Works with Home Assistant program. Those integrations will be flagged as grandfathered into their existing tier.

### Works with Home Assistant

The Works with Home Assistant program is a device review and certification program in which a device can be certified to work with Home Assistant and provide an excellent user experience.

In order for these devices to be able to get certified, the integration used to integrate the device or service must have reached at least the “Gold” tier. The “Gold” tier is specifically designed to provide the best possible user experience possible.

If rules to our quality integration scales have been adjusted, it is possible that an integration with the Works with Home Assistant program doesn’t comply with the newly adjusted standards yet. At that moment, the existing “Gold” tier is grandfathered. Once a new or extra device is certified for the same integration, the integration must be updated to also comply with the new or adjusted standards.

When all devices are removed from the Works with Home Assistant program of a grandfathered integration, the grandfathered flag will also be released and the integration quality scale tier adjusted to the current status of the integration.

## Consequences

The integration quality scale will:
- give a clear and transparent framework for contributors to understand the quality expectations for their integrations.
- put pressure on the maintenance of the developer documentation to make sure the documentation is up-to-date.