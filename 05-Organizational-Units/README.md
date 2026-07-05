# Organizational Units (OUs)

## Executive Summary

Organizational Units (OUs) are containers within Active Directory used to logically organize users, computers, groups, and other directory objects. Unlike the default containers created during domain deployment, OUs provide administrators with the flexibility to structure the directory according to business requirements.

A well-designed OU structure simplifies administration, supports Group Policy deployment, enables delegated administration, and improves the overall scalability of the Active Directory environment.

This section documents the planning, creation, and management of Organizational Units within the **corp.local** domain.

---

# Business Justification

As an Active Directory environment grows, managing all users, computers, and groups from the default containers quickly becomes inefficient.

Implementing a structured OU hierarchy provides several benefits:

- Logical organization of directory objects.
- Simplified administration.
- Targeted Group Policy deployment.
- Delegation of administrative permissions.
- Easier auditing and troubleshooting.
- Improved scalability.

A properly designed OU structure is the foundation of an efficient Active Directory environment.

---

# Objectives

This phase will cover:

- Understanding Organizational Units.
- Planning an OU hierarchy.
- Creating Organizational Units.
- Protecting OUs from accidental deletion.
- Moving objects into OUs.
- Managing Organizational Units.
- Verifying the OU structure.

---

# Proposed OU Structure

The following hierarchy will be implemented throughout this repository:

```text
corp.local
│
├── Administration
│
├── IT
│
├── Human Resources
│
├── Finance
│
├── Sales
│
├── Marketing
│
├── Servers
│
├── Workstations
│
├── Service Accounts
│
└── Security Groups
```

As the repository progresses:

- User accounts will be created inside their respective departmental OUs.
- Client computers will be moved into the **Workstations** OU.
- Member servers will be placed in the **Servers** OU.
- Security groups will be organized within the **Security Groups** OU.
- Group Policy Objects (GPOs) will be linked to the appropriate OUs rather than the entire domain wherever practical.

---

# Why Organizational Units Matter

Organizational Units are more than folders.

They provide the ability to:

- Apply Group Policies to specific departments.
- Delegate administrative permissions without granting Domain Admin privileges.
- Organize objects according to business functions.
- Simplify administration in large environments.
- Reduce the scope of administrative changes.

For example, a password policy or desktop restriction can be applied only to users within the **Finance** OU without affecting users in other departments.

---

# Documents in This Section

| Document | Description |
|----------|-------------|
| Planning-OU-Structure.md | Designing an Organizational Unit hierarchy |
| Create-Organizational-Units.md | Creating Organizational Units using the GUI |
| Create-OUs-PowerShell.md | Creating Organizational Units using PowerShell |
| Managing-Organizational-Units.md | Renaming, moving, deleting, and protecting OUs |
| OU-Validation.md | Verifying the Organizational Unit structure |

---

# Best Practices

- Design the OU hierarchy before creating objects.
- Organize OUs based on administrative requirements rather than mirroring the company's organizational chart exactly.
- Use clear and consistent naming conventions.
- Protect OUs from accidental deletion.
- Apply Group Policy to OUs instead of broad domain-wide deployment whenever possible.
- Delegate permissions at the OU level rather than granting excessive administrative rights.

---

# Expected Outcome

By the end of this phase:

- A structured OU hierarchy will be in place.
- Active Directory objects will be organized logically.
- The environment will be prepared for user management and Group Policy deployment.
- Administrative tasks will become easier to manage and scale.

---

# Conclusion

Organizational Units provide the structural framework for Active Directory administration. A carefully planned OU hierarchy improves manageability, supports delegated administration, and enables precise application of Group Policy Objects.

The OU structure created in this phase will serve as the foundation for the remaining sections of this repository, including user management, group management, and Group Policy configuration.

