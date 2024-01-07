# security-policy-templates

A set of foundational but comprehensive policies, standards and procedures
designed for cloud-native technology organizations. The policy package covers
the requirements and controls for most compliance frameworks and best practices,
in a lightweight approach.

To support its mission of GRC Democratization, [Brainframe.com][brainframe] 
facilitates the use and continious improvement of these templates to help companies
build and run an efficient and effective information security management system
while focussing on standardisation and avoiding costly re-invention of the wheel.

The templates can also be used as stand-alone documents, or in combination with the [`jupiter-policy-builder` CLI][builder]

[builder]: https://github.com/JupiterOne/jupiter-policy-builder
[brainframe]: https://www.brainframe.com

## TL;DR

Run the following command to install the policy builder and build the policies.
You will be prompted for a few inputs, such as company name, to be included in
your policy text.

```bash
npm install -g @jupiterone/jupiter-policy-builder

psp build -t ./templates
```

You will be prompted to save the config to a file, which you can reference the
next time you'd like to rebuild the policies and procedures:

```bash
psp build -t ./templates -c path/to/your/config.json
```

The result files are put in `./docs` (Markdown) and `./site` (HTML).

**IMPORTANT:** To edit the policies and procedures, use the template files in
`./templates` and re-run the `psp build` command. Do _not_ edit the `./docs` and
`./partials` files directly as they will be overwritten on the next build.

For more detailed builder instructions, see the README [here][builder].

## Format

Similar to the concept of "micro-services", the policies and procedures are
written in "micro-docs" that are decoupled from the policies. They are mapped
to each other via a JSON configuration.

- All policies, procedures and reference documents are written in **Markdown**.
- All configuration files are in **JSON** format, including mapping between
  policies and procedures, and between procedures and compliance standards.

## Structure

`templates/policies`

- This directory contains the modular templates for policies.
- Each policy document is written in the following structure:

    ```markdown
    # Policy Title

    `revision`

    Overview of the policy. A paragraph or two to describe the intent and
    principals of the policy.

    ## Policy Statements

    This section contains the high level requirements specific to the policy.

    Policy statements are aligned to the organization's operating model and
    applicable compliance requirements. These statements describe the "what"
    but not the "how". They are meant to be stable over longer periods of time 
    without needing frequent updates.
    ```

`templates/procedures`

- This directory contains the modular templates for controls and procedures.
- Each procedure is considered a "micro-document" that describes how a specific
  control is implemented to address the requirements in the policy that it
  implements.
- Each procedure document is written in the following structure:

    ```markdown
    ### Control/Procedure Title

    Overview. A few lines to describe the control and procedure.

    Detailed text.

    #### Sub-section as needed

    More text.
    ```

  > Note that the **Title** is a _level 3 heading_. This is because the
  `policy-builder` tool will automatically combine the procedures and policies
  they each implement to a single document for publishing, and it will insert a
  `## Controls and Procedures` section heading after the **Policy Statements**
  section and before the first control/procedure.

`templates/ref`

- This directory contains the modular templates for reference documents.
- The reference documents are similar to control procedures. At build/publish
  time, they will be combined and attached to the **Addendum and References**
  document by the `policy-builder`

`templates/assessments`

- This directory contains templates for assessment reports.
- The only available report template at the moment is for a HIPAA risk assessment.
- The `jupiter-policy-builder` CLI tool can leverage this template to
  auto-generate a self assessment report based on the adoption of policies and
  procedures.

## Configuration and Mapping

`config.json { organization }`

There are variables (e.g. `companyFullName`) defined throughout the template
documents. These variables make it easy to adopt and maintain the policy set by
configuring the variables at one place -- in the `organization` section of
`config.json`.

JupiterOne's policy builder (both the web app and CLI) makes use of this
configuration to build the final policy set.

`config.json { policies }`

The `policies` section is a repository of the policy modules. Each entry is
defined with the following:

- `id` - typically matches the filename of the policy without the extension
- `file` - path to the markdown module (without the `.tmpl` extension)
- `name` - policy name, which typically matches the title in the policy doc
- `adopted` - a `boolean` flag to indicate if the policy has been adopted by the
  organization
- `procedures` - an array that contains the procedure modules that
  implement/enforce this policy.

Similar to the concept of "micro-services", the policies and procedures are
written in "micro-docs" that are decoupled from the policies. They are mapped
to each other via the above configuration.

`config.json { procedures }`

The `procedures` section is a repository of the procedure modules. Each entry is
defined with the following:

- `id` - typically matches the filename of the procedures without the extension
- `file` - path to the markdown module (without the `.tmpl` extension)
- `name` - procedure name, which typically matches the title in the procedure doc
- `type` - specifies one of the following procedure types:
  - `administrative`
  - `technical`
  - `operational`
  - `physical`
  - `informative`
- `provider` - name of the control provider (e.g. "AWS IAM")
- `summary` - provides guidance to the document author specific to that procedure
- `applicable` - a `boolean` that specifies whether the procedure is applicable
  to the organization based on its operational and compliance requirements
- `adopted` - a `boolean` flag to indicate if the policy has been adopted by the
  organization

`config.json { references }`

Similar to procedures, the `references` section is a repository of additional
documentation. Each entry is defined with the following:

- `id` - typically matches the filename of the procedures without the extension
- `file` - path to the markdown module (without the `.tmpl` extension)
- `name` - document name, which typically matches the title in the procedure doc
- `applicable` - a `boolean` that specifies whether the document is applicable
  to the organization based on its operational and compliance requirements
- `adopted` - a `boolean` flag to indicate if the policy has been adopted by the
  organization

### Compliance Mapping

`standards/controls-mapping.json`

The `controls-mapping.json` document configures a mapping of each procedure to
one or more security/compliance frameworks, as applicable.

Note that the controls mapping is only between a control/procedure document to
the requirement, not at the policy level. This is because we strongly believe
that you must have documented controls and procedures to implement and enforce a
high level written policy. Having a written policy by itself without 
implementation or enforcement does not address the risk of any security
or compliance requirement.

The JSON documents for those four frameworks are included strictly because of
our internal usage and shown as examples. Using those requires that you have
obtained necessary end-user license for the framework/standard for your own organization.

### Original Repository
This project is a fork of [security-policy-templates][origin] originally developed 
by [JupiterOne][codeowners]. 

[origin]: https://github.com/JupiterOne/security-policy-templates
[codeowners]: https://github.com/JupiterOne/security-policy-templates/blob/main/CODEOWNERS

## License
This work is licensed under the same terms as the original, [Creative Commons Attribution-ShareAlike 4.0 International License (CC-BY-SA-4.0)][CC]. 
A copy of the license is available in the [LICENSE][license] file of this repository.

[CC]: https://creativecommons.org/licenses/by-sa/4.0/
[license]: https://github.com/BrainframeGRC/security-policy-templates/blob/main/LICENSE

## Modifications
Significant modifications and additions have been made for the specific purposes of
this project. Any modifications made in this fork are also shared under the same license.

## Acknowledgements
We acknowledge and thank JupiterOne for their great work on the original version of 
security-policy-templates. Their contributions have been foundational to this project.
