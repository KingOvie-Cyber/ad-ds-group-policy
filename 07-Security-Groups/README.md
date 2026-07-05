# Active Directory Security Groups

## Executive Summary

Security Groups are one of the most fundamental components of Active Directory. They simplify permission management by allowing administrators to assign permissions to groups instead of assigning permissions directly to individual user accounts.

This approach, commonly referred to as **Role-Based Access Control (RBAC)**, improves security, reduces administrative effort, and provides a scalable method of managing access to organizational resources.

Throughout this section, security groups will be created, populated with users, and used as the basis for implementing access control and Group Policy targeting.

---

# Business Justification

Managing permissions at the individual user level quickly becomes impractical as an organization grows. Security Groups allow administrators to grant permissions based on job functions or responsibilities, ensuring consistent access management while reducing the risk of configuration errors.

Implementing Security Groups provides the following benefits:

- Simplified permission management.
- Centralized access control.
- Support for Role-Based Access Control (RBAC).
- Easier auditing and compliance.
- Reduced administrative overhead.
- Improved scalability.
- Consistent access assignments.

---

# Objectives

This section covers:

- Understanding Active Directory Security Groups.
- Understanding Group Types.
- Understanding Group Scopes.
- Creating Security Groups.
- Adding and removing group members.
- Managing group memberships.
- Creating groups using PowerShell.
- Verifying group membership.
- Applying Security Groups in real-world scenarios.

---

# Environment

| Component | Value |
|----------|-------|
| Domain Controller | DC01 |
| Domain | corp.local |
| Management Tool | Active Directory Users and Computers |
| PowerShell Module | ActiveDirectory |

---

# Planned Security Groups

The following Security Groups will be created and used throughout this repository.

| Group Name | Purpose |
|------------|---------|
| IT_Admins | IT administrative staff |
| Finance_Users | Finance department users |
| HR_Users | Human Resources staff |
| Sales_Users | Sales department users |
| Marketing_Users | Marketing department users |
| Workstation_Admins | Local administration of workstations |
| Server_Admins | Administration of member servers |

These groups will later be referenced when configuring:

- File and folder permissions.
- Shared folder access.
- Network resource permissions.
- Group Policy security filtering.
- Administrative delegation.

---

# Group Types

Active Directory supports two group types.

## Security Groups

Security Groups are used to assign permissions to resources such as:

- Shared folders.
- Printers.
- Applications.
- File servers.
- Administrative tasks.

They can also be used for Group Policy security filtering.

---

## Distribution Groups

Distribution Groups are intended for email distribution and cannot be used to assign permissions.

Because this repository focuses on infrastructure administration, only **Security Groups** will be used.

---

# Group Scopes

Selecting the appropriate group scope is important when designing an Active Directory environment.

### Domain Local

Used primarily for assigning permissions to resources located within the same domain.

Example:

```
Finance Shared Folder Access
```

---

### Global

Used to group users with similar job functions or responsibilities.

Example:

```
Finance Employees
IT Administrators
Sales Team
```

Global groups are commonly used in single-domain environments and will be the primary scope used throughout this project.

---

### Universal

Used primarily in multi-domain forests where group memberships need to span multiple domains.

Universal groups simplify access management across domains but require additional replication considerations.

---

# Recommended Design

For this project:

- **Group Type:** Security
- **Group Scope:** Global

This configuration is appropriate for a single-domain Active Directory environment and aligns with common enterprise practices.

---

# Documents in This Section

| Document | Description |
|----------|-------------|
| Create-Security-Groups.md | Creating Security Groups using ADUC and PowerShell |
| Managing-Group-Membership.md | Adding and removing users from groups |
| Group-Validation.md | Verifying group configuration and memberships |

---

# Best Practices

- Assign permissions to groups instead of individual users.
- Use descriptive group names.
- Keep group memberships current.
- Remove unnecessary memberships promptly.
- Follow the principle of least privilege.
- Document the purpose of each group.
- Use consistent naming conventions throughout the environment.

---

# Expected Outcome

By the end of this section:

- Security Groups will be created.
- Users will be assigned to the appropriate groups.
- Role-Based Access Control will be implemented.
- The environment will be prepared for resource permissions and Group Policy filtering.

---

# Conclusion

Security Groups provide a scalable and secure method for managing permissions in Active Directory. By assigning permissions to groups rather than individual users, administrators simplify management, improve security, and establish a strong foundation for enterprise access control.

The groups created during this section will be used extensively in later chapters covering shared resources, delegated administration, and Group Policy.
