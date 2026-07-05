# Active Directory User Management

## Executive Summary

User accounts are one of the most important object types within Active Directory. They provide identities that allow individuals to authenticate to the domain, access organizational resources, receive Group Policy Objects (GPOs), and perform their assigned responsibilities.

This section documents the creation, management, and administration of Active Directory user accounts using both graphical tools and Windows PowerShell. The user accounts created throughout this project will be organized into the Organizational Units established in the previous section.

---

# Business Justification

In an enterprise environment, user accounts should be centrally managed to ensure consistency, security, and compliance.

Centralized user management provides several benefits:

- Centralized authentication.
- Consistent password enforcement.
- Simplified user lifecycle management.
- Improved security auditing.
- Easier permission management.
- Integration with Group Policy.
- Support for role-based access control.

---

# Objectives

This section will demonstrate how to:

- Create user accounts.
- Configure user account properties.
- Reset passwords.
- Force password changes at next logon.
- Disable and enable user accounts.
- Unlock locked user accounts.
- Delete user accounts.
- Move users between Organizational Units.
- Create users using PowerShell.
- Verify user account creation.

---

# Environment

| Component | Value |
|----------|-------|
| Domain Controller | DC01 |
| Domain | corp.local |
| Management Tool | Active Directory Users and Computers |
| PowerShell Module | ActiveDirectory |

---

# User Accounts to Be Created

The following sample accounts will be used throughout the remainder of this repository.

| Username | Department | Organizational Unit |
|----------|------------|---------------------|
| jdoe | IT | IT |
| asmith | Finance | Finance |
| bjohnson | Human Resources | Human Resources |
| cwilson | Sales | Sales |
| dmartin | Marketing | Marketing |

These accounts will later be used when demonstrating:

- Security Groups
- Group Policy
- Password Policies
- Login Restrictions
- Folder Redirection
- Drive Mapping
- Logon Scripts

---

# Documents in This Section

| Document | Description |
|----------|-------------|
| Create-Users.md | Creating user accounts using ADUC and PowerShell |
| Managing-Users.md | Managing existing user accounts |
| Reset-Passwords.md | Password resets and password policies |
| Disable-and-Unlock-Users.md | Disabling, enabling, and unlocking accounts |
| User-Validation.md | Verifying user account configuration |

---

# Best Practices

- Place users in the appropriate Organizational Unit.
- Use a consistent naming convention.
- Avoid using shared user accounts.
- Apply the principle of least privilege.
- Review inactive accounts regularly.
- Disable accounts instead of immediately deleting them when appropriate.
- Document account creation and changes.

---

# Expected Outcome

By the end of this section:

- User accounts will be successfully created.
- Users will be organized into the appropriate Organizational Units.
- Administrative account management tasks will be understood.
- The environment will be prepared for Security Groups and Group Policy deployment.

---

# Conclusion

User accounts form the foundation of identity management within Active Directory. Proper planning, organization, and administration of user accounts improve security, simplify management, and enable centralized access to enterprise resources.

The accounts created during this section will be referenced throughout the remaining chapters of this repository to demonstrate real-world Active Directory administration tasks.
