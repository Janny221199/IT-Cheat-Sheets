
# NACL 
1. **Level of Operation**:
    - NACLs operate at the **subnet level**. This means they apply to all resources within a subnet.
2. **Stateless**:
    - NACLs are **stateless**. This means that if you allow inbound traffic from a particular IP, the corresponding outbound traffic is not automatically allowed. You would need to define separate rules for that.
3. **Rule Evaluation**:
    - Rules in a NACL are evaluated based on the **rule number**. The rules are processed in ascending order, starting from the lowest number.
4. **Number of Rules**:
    - NACLs can have both **allow** and **deny** rules. You can have up to **20 inbound and 20 outbound rules** for each NACL.
5. **Default Action**:
    - When you create a new NACL, by default, it **denies all inbound and outbound traffic**.

# Security Group
1. **Level of Operation**:
    - Security Groups operate at the **instance level**. This means they are assigned to individual EC2 instances or other AWS resources.
2. **Stateful**:
    - Security Groups are **stateful**. If you allow inbound traffic from a specific IP, the corresponding outbound traffic is automatically allowed, regardless of outbound rules.
3. **Rule Evaluation**:
    - Security Groups evaluate rules based on the **most specific rule**. If there's an explicit allow or deny rule that matches the traffic, it takes precedence.
4. **Number of Rules**:
    - Security Groups can have **inbound and outbound rules**, but they have **higher limits** than NACLs. By default, each Security Group can have **60 inbound and 60 outbound rules**.
5. **Default Action**:
    - When you create a new Security Group, by default, it **allows all outbound traffic** and **denies all inbound traffic**.