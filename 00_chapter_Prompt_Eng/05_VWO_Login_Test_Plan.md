# Test Plan ID: TP-VWO-LOGIN-001

## 1. Test Plan Title

VWO.COM Login Module — Positive, Negative, and Regression Testing for QA Environment

## 2. Objective

Validate that users can successfully sign in to VWO.COM and that invalid, missing, and malformed login inputs are handled safely and consistently. The plan supports identification and reporting of application defects and validates the existing Selenium 4 / Java 17 / TestNG automation framework against the login module.

## 3. References

- Application: VWO.COM
- Feature: Login Module
- Business domain: Salesforce
- Target environment: QAT
- Default application URL: `https://login.salesforce.com`
- Existing automated implementation: Selenium 4.27.0, Java 17, TestNG 7.10.2, Maven Surefire 3.5.2, ExtentReports 5.1.2
- Existing login page object: LoginPage
- Existing login test class: LoginTest

> The URL and framework configuration were confirmed from the repository. Credentials and other sensitive values must not be copied into planning or test-result artifacts.

## 4. In Scope

- Login page rendering and availability.
- Successful authentication with an approved valid test account.
- Invalid username or password handling.
- Empty and whitespace-only credential handling.
- Basic form validation and error-message behavior.
- Authentication flow regression coverage through the existing login page object.
- Manual QA execution for the same scenarios.
- Failure screenshots, logs, and report evidence for failed test steps.
- Defect reporting and triage for failures with severity and priority metadata.

## 5. Out of Scope

- Production environment validation.
- Full Salesforce organization configuration and admin-user role validation.
- Password recovery, multi-factor authentication, SSO, and account lockout behavior unless explicitly added to scope.
- Performance, load, accessibility, localization, and cross-browser compatibility testing unless approved separately.
- End-to-end business workflows after successful login, such as lead management, unless added to this plan.
- Changes to application code or test infrastructure that are not required for executing the login scenarios.

## 6. Requirements and Planned Coverage

| Requirement ID | Requirement | Planned Coverage | Evidence / Status |
|---|---|---|---|
| REQ-LOGIN-001 | Login page loads in QAT and displays the login controls. | Manual and automated rendering checks | Confirmed by existing LoginTest; execution not yet performed |
| REQ-LOGIN-002 | Valid credentials allow the user to authenticate and enter the post-login experience. | Manual and automated successful-login scenario | Existing page-object flow available; test case to be added or confirmed |
| REQ-LOGIN-003 | Invalid username or password is rejected and an error is shown. | Manual and automated negative scenario | Existing invalid-credential test present; execution not yet performed |
| REQ-LOGIN-004 | Empty or whitespace-only credentials do not authenticate and do not produce an unexpected application error. | Manual and automated validation scenario | Existing empty-credential test present; execution not yet performed |
| REQ-LOGIN-005 | Login inputs do not display or persist sensitive credential values in logs, screenshots, or reports. | Manual and automated security review | Proposed validation; no reporting evidence supplied |
| REQ-LOGIN-006 | Failure results include enough evidence to reproduce and diagnose the defect. | Automated failure reporting and manual defect report | Reporting framework available; execution not yet performed |

## 7. Test Approach

- **Manual testing:** Use an approved test account and repeat each scenario in the QAT browser.
- **Automation:** Use the existing Maven, Selenium, TestNG, and ExtentReports configuration.
- **Test levels:** Smoke, regression, and targeted functional validation.
- **Test types:** Functional, integration, negative, boundary, and regression.
- **Execution order:** Verify page rendering first, then successful authentication, followed by invalid and empty credential cases.
- **Isolation:** Each test starts with a fresh browser session and uses approved synthetic or environment-provided test data.
- **Evidence:** Record test ID, action, expected result, actual result, execution status, logs, and failure screenshots.

## 8. Test Environment

| Item | Value |
|---|---|
| Environment | QAT |
| Application URL | `https://login.salesforce.com` |
| Browser | Chrome by default; Firefox or Edge may be used when configured |
| Runtime | Java 17 LTS |
| Automation framework | Selenium 4.27.0 |
| Test runner | TestNG 7.10.2 |
| Build tool | Maven |
| Report format | ExtentReports HTML |
| Browser mode | Default Chrome non-headless; smoke suite configured headless |
| Test account | Approved CRM test account; secrets must remain outside source control |
| Test data | Approved synthetic or environment-provided values only |

## 9. Test Data and Prerequisites

- A valid approved test account must be available for the successful-login scenario.
- A known-invalid account or password must be available for the negative scenario.
- The QAT endpoint must be reachable from the test workstation or CI runner.
- Browser automation tooling must be installed and compatible with the configured Selenium version.
- The test runner must have permission to execute the Maven project and write reports.
- The test account must not be used for unrelated production or customer data.
- No credentials are to be hard-coded in test source files, plan artifacts, screenshots, or reports.

> Exact account names, passwords, tokens, and URLs for non-public access must be supplied through an approved secret mechanism. Account availability and credential validity were not confirmed in the repository inputs.

## 10. Test Cases

### Manual and Automated Login Cases

| Test ID | Requirement ID | Scenario | Type | Priority | Preconditions | Test Data | Actions | Expected Result | Actual Result / Status |
|---|---|---|---|---|---|---|---|---|---|
| TC-LOGIN-001 | REQ-LOGIN-001 | Login page rendering | Smoke / Functional | P1 | QAT reachable; browser available | N/A | Open the login URL and inspect the page. | Login form, username field, password field, and submit control are visible; page title identifies the login portal. | Not executed |
| TC-LOGIN-002 | REQ-LOGIN-002 | Successful login | Functional / Regression | P1 | Valid approved account; QAT reachable | Valid test account | Enter username and password, submit, wait for the post-login page. | Authentication succeeds and the expected post-login destination or dashboard is displayed without an error. | Not executed |
| TC-LOGIN-003 | REQ-LOGIN-003 | Invalid username or password | Negative | P1 | QAT reachable; invalid account or password available | Invalid credential pair | Enter an invalid username or password, submit, inspect the page. | Login is rejected; no authenticated session is established; a user-readable error is displayed. | Not executed |
| TC-LOGIN-004 | REQ-LOGIN-004 | Empty credentials | Boundary / Negative | P1 | QAT reachable | Empty strings | Submit with empty username and password. | Login is not completed; validation or an error message is shown; the user remains on the login page. | Not executed |
| TC-LOGIN-005 | REQ-LOGIN-004 | Whitespace-only credentials | Boundary / Negative | P2 | QAT reachable | Spaces or tabs | Enter whitespace-only values and submit. | Login is not completed; validation or an error message is shown without exposing credentials. | Not executed |
| TC-LOGIN-006 | REQ-LOGIN-003 | Wrong credential format | Negative | P2 | QAT reachable | Malformed but non-empty test values | Enter a malformed username format and a valid-looking password, submit. | The application rejects the input with the expected validation or authentication response; no authenticated session is created. | Not executed |
| TC-LOGIN-007 | REQ-LOGIN-005 | Credential data handling | Security / Regression | P1 | QAT reachable; test-runner access | Synthetic test values | Submit credentials and inspect automated logs, report artifacts, screenshots, and browser state. | Credentials are absent from logs, reports, screenshots, and visible session artifacts. | Not executed |
| TC-LOGIN-008 | REQ-LOGIN-006 | Failed-test evidence | Defect Management | P1 | QAT reachable; test failure available | Depends on failed scenario | Execute a failing scenario and inspect the generated report. | Failure evidence includes the test ID, action, expected result, actual result, error details, and screenshot where applicable. | Not executed |

## 11. Automation Execution Plan

- Use the existing `LoginTest` class and `LoginPage` page object.
- Run the smoke suite through the configured Maven command:

```bash
mvn clean test -Dbrowser=chrome -Denv=qa -Dheadless=true
```

- Keep the current smoke XML suite behavior as the baseline.
- Add or update the successful-login positive test only after a valid approved account is available.
- Add negative cases for invalid, empty, whitespace-only, and malformed credentials where the application contract confirms the expected outcome.
- Do not assert exact error text unless that text is supplied or verified from the application.
- Do not use fixed sleeps; use explicit synchronization and observable conditions.
- Use existing retries only for the framework's defined transient failure policy and do not hide assertion or authentication failures.
- Record actual test execution, pass/fail status, duration, and report location.

## 12. Entry Criteria

- The QAT login URL is reachable.
- A valid approved account is available for the positive scenario.
- The configured browser and Selenium driver are installed.
- The repository dependencies can resolve and Maven can compile the test project.
- Negative test data is available or can be generated without using production credentials.
- The test owner and reviewer have been identified.

## 13. Exit Criteria

- All P1 cases are executed or an approved deferral is documented.
- Each executed case has a recorded actual result and execution status.
- The test report contains the expected test IDs, steps, results, and failure evidence.
- All P1 or P2 defects are triaged and assigned to an owner.
- No unresolved critical security issue remains from credential handling checks.
- A final summary identifies completed, blocked, deferred, and not-run work.

> “All applicable tests pass” must not be claimed without fresh execution evidence and an inspected report.

## 14. Roles and Responsibilities

| Role | Responsibility |
|---|---|
| QA Lead | Approves scope, priorities, environment, and exit criteria |
| QA Tester | Executes manual and automated cases and records actual results |
| Automation Engineer | Maintains Selenium/TestNG page objects and test suite |
| Application Owner | Confirms application behavior, valid test accounts, and acceptance criteria |
| Defect Manager | Triage, assign, and monitor defects |
| Release Manager | Confirms whether any unresolved defects block release |

## 15. Defect Reporting

For every failed case, record:

- Defect ID and title.
- Test ID and requirement ID.
- Environment and browser.
- Preconditions and test data used.
- Steps to reproduce.
- Expected result.
- Actual result.
- Screenshot or log evidence.
- Reproduction status.
- Severity and priority.
- Affected component and release impact.
- Assigned owner and target resolution date.

Use severity and priority definitions agreed by the organization. Until confirmed, severity and priority must be labeled as proposed rather than final.

## 16. Risks, Dependencies, and Assumptions

| Type | Item | Impact | Mitigation / Follow-up |
|---|---|---|---|
| Assumption | VWO.COM login uses the Salesforce login endpoint in QAT | If the endpoint differs, automation may fail | Confirm URL and application owner before execution |
| Assumption | The existing login page object locators represent the intended QAT login UI | Incorrect locators may produce false failures | Validate locators manually and update them from confirmed UI evidence |
| Assumption | A valid approved account is available | Successful-login coverage cannot be executed | Provide an approved test account through the existing secret mechanism |
| Dependency | Browser and Selenium driver availability | Automatic execution may be blocked | Verify Java, browser, driver, and Maven prerequisites |
| Dependency | QAT network access | Tests may be blocked | Confirm connectivity and firewall rules |
| Risk | Credentials in repository configuration | Security exposure | Move credentials to approved secret storage and confirm they are excluded from reports |
| Risk | Existing invalid-credential assertion is overly broad | Test could pass for the wrong reason | Confirm the intended visible error and use stable assertions |
| Risk | Without a stated release deadline | Exit timing cannot be measured | Agree on target date and required test completion window |

## 17. Open Questions

1. What exact QAT URL and application version should be used for this test plan?
2. Which valid account should be used for the successful-login scenario?
3. Are password recovery, SSO, MFA, and account lockout behavior required in scope?
4. Which browsers and operating systems must be covered?
5. What are the required severity, priority, and defect SLA values?
6. What is the target execution date or release gate?
7. Should the existing smoke suite be expanded or should a dedicated login suite be created?
8. Are screenshots and logs required for every successful case, only failures, or both?

## 18. Suspension and Resumption Criteria

- **Suspend:** QAT is unavailable, a required account is unavailable, the URL or browser configuration is invalid, or a critical application defect prevents safe execution.
- **Resume:** The affected prerequisite is restored, the application owner approves the corrected environment, and the QA lead confirms that the plan can be executed safely.
- **Reassess:** If the application contract, environment, or scope changes materially, update the affected requirements and test cases before continuing.

## 19. Deliverables

- This approved test plan.
- Test data management notes without credential values.
- Automated login test cases and supporting page-object changes.
- Maven/TestNG execution command and report location.
- Manual execution record.
- Defect report with evidence and triage status.
- Final test summary with execution counts, defects, and unresolved items.

## 20. Approval

- **Plan status:** Draft — awaiting completion of open questions and required account/environment approval.
- **Plan approval:** Required under the Ricepot template.
- **Execution checkpoints:** After plan approval and after each major work item.
- **Final validation:** Review requirement coverage, test data, repository configuration, and actual execution evidence before declaring completion or release readiness.
