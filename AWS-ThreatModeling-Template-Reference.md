# AWS Threat Modeling Template Reference

This document provides a comprehensive reference for building an AWS threat modeling template in Microsoft Threat Modeling Tool.

## 1. AWS Service Stencils by Category

### Compute Services
| Service Name | Description | Threats | Mitigations |
|-------------|-------------|---------|-------------|
| Amazon EC2 | Virtual servers in the cloud | S: Credential theft<br>T: Instance metadata manipulation<br>R: Disabled logging<br>I: Exposed instance data<br>D: Resource exhaustion<br>E: Instance profile abuse | - Security groups with minimal inbound rules<br>- Regular patching<br>- IMDSv2 implementation<br>- Host-based intrusion detection |
| Amazon EC2 Auto Scaling | Scale EC2 capacity automatically | T: Auto-scaling policy manipulation<br>D: Auto-scaling misconfiguration | - Protected scaling policies<br>- Scale-in protection<br>- Monitoring of scaling events |
| Amazon ECR | Container registry | S: Container image impersonation<br>T: Container image manipulation<br>I: Sensitive data in images | - Image scanning<br>- Signing requirements<br>- Immutable tags |
| Amazon ECS | Container orchestration | S: Task role abuse<br>T: Task definition manipulation<br>E: Container escape | - Task role least privilege<br>- Protected task definitions<br>- Runtime monitoring |
| Amazon EKS | Managed Kubernetes | S: Role spoofing<br>T: Pod tampering<br>E: Container escape | - RBAC controls<br>- Pod security policies<br>- Network policies |
| AWS Lambda | Serverless functions | T: Function code injection<br>I: Sensitive env variables<br>E: Overprivileged role | - Function immutability<br>- Environment variable encryption<br>- Function role least privilege |
| Amazon Lightsail | Simplified VPS | S: SSH key compromise<br>T: Instance tampering | - Secure key management<br>- Proper firewall configuration |
| AWS Batch | Batch computing | T: Job definition tampering<br>D: Queue flooding | - Protected job definitions<br>- Queue throttling |
| AWS Elastic Beanstalk | Application deployment | T: Environment configuration tampering<br>I: Application secrets exposure | - Protected environment configurations<br>- Secrets encryption |
| AWS Fargate | Serverless containers | S: Task role abuse<br>E: Container escape | - Task role least privilege<br>- Container hardening |
| AWS Outposts | Hybrid infrastructure | S: Local credential theft<br>T: Hardware tampering | - Physical security<br>- Access controls |
| AWS App Runner | Containerized app service | T: Source code tampering<br>I: Configuration exposure | - Source control protection<br>- Configuration encryption |
| AWS Serverless App Repository | Serverless app discovery | T: Application template manipulation<br>E: Excessive permissions | - Template validation<br>- Permission boundaries |

### Storage Services
| Service Name | Description | Threats | Mitigations |
|-------------|-------------|---------|-------------|
| Amazon S3 | Object storage | S: Pre-signed URL abuse<br>T: Bucket policy modification<br>R: Disabled access logging<br>I: Public bucket access<br>D: Bucket flooding<br>E: S3 policy misconfiguration | - Block public access<br>- Bucket policy audit<br>- Access logging<br>- Lifecycle policies<br>- Default encryption |
| Amazon EBS | Block storage | T: Volume data tampering<br>I: Unencrypted snapshots<br>E: Shared snapshots | - Default encryption<br>- Snapshot access control<br>- Regular monitoring |
| Amazon EFS | File storage | T: File system tampering<br>I: Excessive mount targets<br>E: Mount target misconfig | - Encryption in transit/at rest<br>- Access point restrictions<br>- Security group controls |
| Amazon FSx | File systems | T: File system tampering<br>I: Excessive sharing | - Encryption<br>- Access controls<br>- Monitoring |
| Amazon S3 Glacier | Archive storage | T: Vault lock policy tampering<br>I: Retrieval policy exposure | - Vault lock policies<br>- IAM controls<br>- Inventory monitoring |
| AWS Storage Gateway | Hybrid storage | S: Gateway credential theft<br>T: Gateway configuration tampering | - Gateway credential protection<br>- Network security |
| AWS Backup | Centralized backup | T: Backup plan tampering<br>I: Shared backup access | - Backup plan protection<br>- Access controls |
| AWS Snow Family | Edge computing | S: Device credential theft<br>T: Physical tampering | - Physical security<br>- Job manifest validation |

### Database Services
| Service Name | Description | Threats | Mitigations |
|-------------|-------------|---------|-------------|
| Amazon RDS | Relational database | S: Master credential theft<br>T: Parameter group tampering<br>R: Audit logging disabled<br>I: Snapshot exposure<br>D: Connection flooding<br>E: Database privilege escalation | - Multi-factor auth<br>- Encrypted connections<br>- Audit logging<br>- Private snapshots<br>- Connection limits |
| Amazon DynamoDB | NoSQL database | T: Item manipulation<br>R: Streams disabled<br>I: Excessive IAM permissions<br>D: Throughput exceeded | - Point-in-time recovery<br>- Fine-grained access control<br>- On-demand capacity |
| Amazon ElastiCache | In-memory caching | S: AUTH token theft<br>T: Parameter group manipulation<br>I: Redis commands exposure | - Transit encryption<br>- AUTH tokens<br>- Security groups |
| Amazon DocumentDB | Document database | S: Master credential theft<br>I: Snapshot exposure<br>E: Role escalation | - Encryption<br>- Audit logging<br>- Access controls |
| Amazon Keyspaces | Cassandra-compatible | T: Table tampering<br>I: Excessive IAM permissions | - Encryption<br>- Access controls |
| Amazon Neptune | Graph database | S: Master credential theft<br>I: Snapshot exposure | - IAM auth<br>- Encryption<br>- Private endpoints |
| Amazon QLDB | Ledger database | T: Journal tampering attempts | - Cryptographic verification<br>- Journal encryption |
| Amazon Timestream | Time series database | T: Data manipulation<br>I: Query exposure | - Access controls<br>- Encryption |
| Amazon Redshift | Data warehouse | S: Master credential theft<br>I: Cluster endpoint exposure<br>E: Excessive permissions | - Enhanced VPC routing<br>- Encryption<br>- Audit logging |
| Amazon Aurora | MySQL/PostgreSQL | S: Master credential theft<br>I: Snapshot exposure<br>E: Database role escalation | - IAM auth<br>- Encryption<br>- Activity monitoring |
| Amazon MemoryDB for Redis | Durable Redis | S: AUTH token theft<br>I: Redis commands exposure | - Access control lists<br>- Encryption |

### Networking & Content Delivery
| Service Name | Description | Threats | Mitigations |
|-------------|-------------|---------|-------------|
| Amazon VPC | Virtual network | T: Route table manipulation<br>I: NACL/SG misconfiguration<br>D: ARP/IP spoofing | - Protected route tables<br>- SG/NACL monitoring<br>- Flow logs<br>- Traffic inspection |
| Amazon CloudFront | CDN | S: Origin access identity theft<br>T: Distribution config tampering<br>D: Origin overload | - Origin access identities<br>- Field-level encryption<br>- WAF integration |
| Amazon Route 53 | DNS | S: Domain hijacking<br>T: Record set manipulation<br>D: DNS amplification | - DNSSEC<br>- Record set protection<br>- Health checks |
| AWS Direct Connect | Dedicated connections | S: BGP session hijacking<br>T: Virtual interface manipulation<br>D: Connection flooding | - BGP authentication<br>- AS path verification<br>- Traffic monitoring |
| AWS Global Accelerator | Network performance | T: Accelerator configuration tampering<br>D: Endpoint flooding | - Shield integration<br>- Health checks |
| AWS Transit Gateway | Network transit hub | T: Route table manipulation<br>D: Cross-VPC attacks | - Route table protection<br>- Attachment controls |
| Amazon API Gateway | API management | S: API key theft<br>T: API config tampering<br>I: API secrets exposure<br>D: Request flooding | - Lambda authorizers<br>- Request validation<br>- Throttling<br>- WAF integration |
| AWS PrivateLink | Private connectivity | T: Endpoint service configuration<br>I: Unintended access | - Endpoint policies<br>- Service control |
| AWS App Mesh | Service mesh | S: Identity spoofing<br>T: Mesh configuration tampering | - TLS encryption<br>- IAM authorization |
| AWS Cloud Map | Service discovery | T: Service registry tampering<br>I: Service information exposure | - IAM policies<br>- Resource-based policies |
| Elastic Load Balancing | Load balancer | S: SSL certificate theft<br>T: Listener configuration<br>D: Connection flooding | - TLS termination<br>- Security policies<br>- Shield integration |
| Network Firewall | Network filtering | T: Rule configuration tampering<br>I: Firewall bypass | - Protected rule groups<br>- Stateful inspection |
| AWS VPN | Virtual private network | S: Certificate theft<br>T: Connection configuration<br>I: Plaintext transmission | - Certificate rotation<br>- Tunnel encryption<br>- Monitoring |

### Security, Identity & Compliance
| Service Name | Description | Threats | Mitigations |
|-------------|-------------|---------|-------------|
| AWS IAM | Identity management | S: Credential theft<br>T: Policy manipulation<br>R: Deleted policy changes<br>I: Excessive permissions<br>E: Privilege escalation | - MFA<br>- Least privilege<br>- Policy conditions<br>- Permission boundaries<br>- Key rotation |
| Amazon Cognito | User directory | S: User pool credential theft<br>T: Identity pool manipulation<br>I: Unauthorized access | - MFA<br>- Identity verification<br>- Token expiration |
| AWS WAF | Web application firewall | T: Rule manipulation<br>I: Rule bypass | - Protected rule sets<br>- Logging<br>- Monitoring |
| AWS Shield | DDoS protection | T: Protection configuration<br>D: Unmitigated attack vectors | - Automatic protection<br>- Advanced protection |
| AWS Firewall Manager | Centralized firewall | T: Security policy tampering<br>I: Policy bypass | - Organizational policies<br>- Compliance monitoring |
| AWS Secrets Manager | Secrets storage | S: Secret value theft<br>T: Secret version tampering<br>I: Excessive access | - Rotation<br>- Encryption<br>- Resource policies |
| Amazon GuardDuty | Threat detection | T: Detector tampering<br>R: Finding suppression | - Organization-level enablement<br>- Finding protection |
| Amazon Inspector | Vulnerability assessment | T: Assessment tampering<br>R: Finding suppression | - Continuous scanning<br>- Integration with Security Hub |
| Amazon Macie | Data security | T: Classification tampering<br>R: Finding suppression | - Automated scanning<br>- Organization-level control |
| AWS KMS | Key management | S: Key material theft<br>T: Key policy tampering<br>E: Excessive key permissions | - CMK rotation<br>- Key policies<br>- Monitoring |
| AWS Certificate Manager | Certificate management | S: Private key exposure<br>T: Certificate manipulation | - Managed renewal<br>- Private CA |
| AWS Directory Service | Managed directories | S: Directory credential theft<br>E: Directory privilege escalation | - Password policies<br>- MFA<br>- Conditional access |
| AWS Single Sign-On | Centralized access | S: SSO credential theft<br>E: Permission set abuse | - MFA<br>- Session duration<br>- Permission set review |
| AWS Security Hub | Security posture | T: Standard tampering<br>R: Finding suppression | - Organizational enablement<br>- Automated remediation |
| AWS CloudHSM | Hardware security module | S: HSM credential theft<br>T: HSM configuration tampering | - Quorum authentication<br>- Network isolation |
| Amazon Detective | Security investigation | T: Graph tampering<br>R: Finding deletion | - Organization-level enablement<br>- Integration with GuardDuty |

### Management & Governance
| Service Name | Description | Threats | Mitigations |
|-------------|-------------|---------|-------------|
| AWS CloudFormation | Infrastructure as code | T: Template manipulation<br>I: Secrets in templates<br>E: Resource role abuse | - Template validation<br>- Drift detection<br>- Stack policies |
| AWS CloudTrail | Activity tracking | T: Trail configuration tampering<br>R: Log file tampering | - Log file validation<br>- Organization trails<br>- S3 MFA delete |
| Amazon CloudWatch | Monitoring | T: Alarm manipulation<br>R: Log manipulation<br>I: Sensitive log data | - Log encryption<br>- Metric alarm protection<br>- IAM policy conditions |
| AWS Config | Resource inventory | T: Rule tampering<br>R: Configuration history deletion | - Organization rules<br>- Remediation protection |
| AWS Organizations | Account management | T: Policy tampering<br>E: Service control policy bypass | - SCP monitoring<br>- Management account protection |
| AWS Systems Manager | Operations management | S: Parameter Store value theft<br>T: Document tampering<br>E: Session Manager abuse | - Parameter encryption<br>- Session logging<br>- Document versioning |
| AWS Control Tower | Landing zone | T: Control tampering<br>R: Drift suppression | - Mandatory controls<br>- Drift detection |
| AWS License Manager | License management | T: License rule tampering<br>I: License usage exposure | - Rule protection<br>- Usage tracking |
| AWS Trusted Advisor | Best practice checks | T: Check tampering<br>I: Check results exposure | - Check prioritization<br>- Result protection |
| AWS Service Catalog | Service deployment | T: Portfolio tampering<br>E: Launch constraint bypass | - Portfolio protection<br>- Launch constraints |
| AWS OpsWorks | Configuration management | S: App credential theft<br>T: Stack configuration tampering | - Credential encryption<br>- Configuration protection |
| AWS Proton | Microservice deployment | T: Template tampering<br>I: Environment exposure | - Template protection<br>- Environment isolation |
| AWS Health Dashboard | Service health | I: Health event exposure | - Event permissions<br>- Event aggregation |

### Analytics Services
| Service Name | Description | Threats | Mitigations |
|-------------|-------------|---------|-------------|
| Amazon Athena | Query service | I: Query results exposure<br>E: Excessive database permissions | - Result encryption<br>- Lake Formation integration |
| Amazon EMR | Hadoop framework | S: Cluster credential theft<br>I: Cluster data exposure | - Kerberos authentication<br>- Encryption<br>- VPC isolation |
| Amazon CloudSearch | Search service | I: Search domain exposure<br>D: Query flooding | - VPC domains<br>- Query throttling |
| Amazon OpenSearch | Search and analytics | S: Domain credential theft<br>I: Data exposure<br>D: Query flooding | - Fine-grained access control<br>- VPC domains<br>- Query limits |
| Amazon Kinesis | Data streaming | T: Stream tampering<br>I: Record exposure<br>D: Shard capacity exceeded | - Server-side encryption<br>- Enhanced fan-out<br>- On-demand capacity |
| Amazon QuickSight | Business intelligence | I: Dashboard exposure<br>E: Data source access | - Row-level security<br>- Private VPC connections |
| AWS Data Pipeline | Workflow orchestration | T: Pipeline definition tampering<br>E: Pipeline role abuse | - Pipeline definition protection<br>- Role least privilege |
| AWS Glue | ETL service | S: Data catalog credential theft<br>T: Job tampering<br>I: Connection information exposure | - Connection password encryption<br>- Job bookmarks<br>- Network configuration |
| AWS Lake Formation | Data lake | T: Permission tampering<br>I: Table exposure<br>E: LF-Tag abuse | - Data lake administrators<br>- Column-level security<br>- Tag-based access control |
| Amazon Data Firehose | Data delivery | T: Delivery stream tampering<br>I: Record exposure | - Server-side encryption<br>- VPC delivery |
| Amazon MSK | Managed Kafka | S: Cluster authentication<br>T: Topic manipulation<br>I: Data exposure | - Encryption<br>- ACL controls<br>- VPC isolation |
| AWS Data Exchange | Data marketplace | I: Subscriber information exposure<br>T: Asset manipulation | - Authorized access<br>- Revision history |
| Amazon FinSpace | Financial data | I: Dataset exposure<br>E: Permission abuse | - Fine-grained permissions<br>- Data lineage |

### Application Integration
| Service Name | Description | Threats | Mitigations |
|-------------|-------------|---------|-------------|
| AWS Step Functions | Workflow orchestration | T: State machine tampering<br>I: Execution data exposure<br>E: IAM execution role abuse | - X-Ray tracing<br>- Execution role least privilege<br>- State machine versioning |
| Amazon AppFlow | Integration service | S: Connection credential theft<br>I: Flow data exposure | - Connection encryption<br>- Private VPC endpoints |
| Amazon EventBridge | Event bus | T: Rule tampering<br>I: Event data exposure<br>D: Event flooding | - Archive and replay<br>- Resource-based policies<br>- Throttling |
| Amazon MQ | Message broker | S: Broker credential theft<br>I: Message exposure<br>D: Queue flooding | - Network isolation<br>- TLS encryption<br>- Authentication |
| Amazon SQS | Message queuing | T: Queue policy tampering<br>I: Message exposure<br>D: Queue flooding | - Server-side encryption<br>- Resource policies<br>- Dead-letter queues |
| Amazon SNS | Pub/sub messaging | T: Topic policy tampering<br>I: Message exposure<br>D: Topic flooding | - Topic policies<br>- Message encryption<br>- Delivery status |
| AWS AppSync | GraphQL APIs | S: API key theft<br>T: Schema tampering<br>I: Data exposure<br>D: Query flooding | - JWT authorization<br>- Resolver mapping templates<br>- Query depth limiting |
| Amazon Managed Workflows for Apache Airflow | Workflow orchestration | S: Airflow credential theft<br>T: DAG tampering<br>E: Execution role abuse | - IAM authentication<br>- VPC isolation<br>- Environment separation |

### Machine Learning & AI
| Service Name | Description | Threats | Mitigations |
|-------------|-------------|---------|-------------|
| Amazon SageMaker | ML platform | S: Notebook credential theft<br>T: Model tampering<br>I: Training data exposure<br>E: Execution role abuse | - VPC isolation<br>- Model encryption<br>- Training job monitoring<br>- IAM role least privilege |
| Amazon Comprehend | NLP service | I: Document exposure<br>E: Custom entity recognition abuse | - Data encryption<br>- VPC endpoints<br>- Access controls |
| Amazon Forecast | Time-series forecasting | I: Dataset exposure<br>T: Predictor tampering | - Data encryption<br>- IAM policies |
| Amazon Lex | Conversational interfaces | I: Utterance exposure<br>T: Bot tampering | - Encryption<br>- Resource policies |
| Amazon Polly | Text-to-speech | I: Input text exposure | - IAM policies<br>- Request throttling |
| Amazon Rekognition | Image/video analysis | I: Image data exposure<br>E: Collection abuse | - Data encryption<br>- Collection access controls |
| Amazon Textract | Document extraction | I: Document exposure | - IAM policies<br>- VPC endpoints |
| Amazon Transcribe | Speech-to-text | I: Audio exposure<br>T: Vocabulary tampering | - Data encryption<br>- Custom vocabulary protection |
| Amazon Translate | Translation | I: Text exposure | - IAM policies<br>- Request throttling |
| Amazon Personalize | Recommendation | I: Dataset exposure<br>T: Campaign tampering | - Data encryption<br>- Resource isolation |
| Amazon Augmented AI | Human review | I: Human reviewer exposure<br>T: Flow definition tampering | - Worker task template restrictions<br>- Flow definition access controls |
| Amazon CodeGuru | Code reviews | I: Code exposure<br>T: Profiling group tampering | - Repository connection security<br>- Profiling group permissions |
| Amazon Fraud Detector | Fraud detection | I: Dataset exposure<br>T: Detector tampering | - Data encryption<br>- Variable type restrictions |
| Amazon Kendra | Enterprise search | I: Index exposure<br>T: FAQs tampering | - Index access control<br>- IAM authentication |
| AWS Deep Learning AMIs | ML development | S: AMI credential theft<br>I: Model exposure | - Key pair restriction<br>- Network isolation |
| Amazon Monitron | Equipment monitoring | I: Measurement exposure<br>T: Project tampering | - Project access controls<br>- User permissions |

### Media, Migration & IoT Services
| Service Name | Description | Threats | Mitigations |
|-------------|-------------|---------|-------------|
| Amazon Elastic Transcoder | Media transcoder | I: Pipeline exposure<br>T: Preset tampering | - Pipeline access controls<br>- Preset protection |
| AWS Migration Hub | Migration tracking | I: Migration task exposure<br>T: Strategy tampering | - IAM policies<br>- Strategy access controls |
| AWS IoT Core | IoT connectivity | S: Device certificate theft<br>T: Policy tampering<br>I: Message exposure<br>D: Message flooding | - Certificate rotation<br>- Device defender<br>- Message encryption<br>- Throttling |
| AWS Blockchain | Distributed ledger | T: Network tampering<br>I: Member information exposure<br>E: Channel access | - Private networks<br>- Member invitation control<br>- Channel access control |

## 2. AWS-Specific Threats Based on STRIDE Model

### Spoofing Threats
- IAM credential theft or unauthorized access
- Hijacking of AWS account credentials
- API key compromise
- Session token hijacking
- Cross-account role assumption attacks
- Domain spoofing of AWS console or APIs
- Container image impersonation
- Role/identity spoofing within EKS/ECS clusters

### Tampering Threats
- S3 bucket policy modification
- CloudFormation template manipulation
- Lambda function code injection
- EC2 instance metadata manipulation
- EBS volume data tampering
- Route table modification for traffic hijacking
- DynamoDB item manipulation
- Unauthorized CloudWatch alarm modifications
- CI/CD pipeline tampering affecting AWS deployments

### Repudiation Threats
- CloudTrail logging disabled or tampered
- VPC Flow Logs disabled or deleted
- S3 access logging disabled
- RDS audit logging disabled
- Lambda execution logging disabled
- API Gateway access logging disabled
- Load Balancer access logs disabled
- CloudFront access logging disabled

### Information Disclosure Threats
- S3 bucket misconfiguration leading to public access
- Overly permissive IAM policies
- EBS snapshots shared inappropriately
- RDS snapshots exposed
- Secrets exposed in Lambda environment variables
- EC2 instance profile with excessive permissions
- Sensitive data in CloudWatch Logs
- Unencrypted data in transit between AWS services
- Cross-tenant data leakage in shared AWS resources

### Denial of Service Threats
- API rate limiting bypassed
- Resource exhaustion via auto-scaling misconfiguration
- Lambda concurrent execution limits reached
- DynamoDB throughput capacity exceeded
- SQS queue flooding
- SNS topic flooding
- Route 53 DNS amplification attacks
- VPC endpoint service resource exhaustion
- CloudFront origin overload

### Elevation of Privilege Threats
- IAM privilege escalation paths
- Role chaining to gain higher permissions
- EC2 instance profile permissions abuse
- Container escape in ECS/EKS
- Lambda function role overprivileged
- Service-linked role abuse
- Resource-based policy misconfigurations
- Cross-account resource access abuse

## 3. AWS-Specific Mitigations

### IAM and Identity Security
- Implement least privilege principle in IAM policies
- Use IAM Access Analyzer to review permissions
- Implement regular IAM policy audits
- Use Service Control Policies (SCPs) to limit permissions
- Implement permission boundaries
- Enable multi-factor authentication for all users
- Enforce strong password policies
- Use temporary credentials when possible
- Implement identity federation
- Rotate access keys regularly

### Network Security
- VPC security groups with minimal access rules
- Network ACLs as additional protection layer
- Use VPC endpoints for service access
- Implement AWS PrivateLink for service isolation
- Transit Gateway with proper route table segmentation
- Use GuardDuty for network threat detection
- Implement appropriate subnet segmentation
- Use AWS Shield for DDoS protection

### Data Protection
- S3 bucket policies restricting public access
- Enable default encryption for S3, EBS, RDS
- Use AWS KMS for key management
- Enable versioning for critical S3 buckets
- Implement lifecycle policies for data tiers
- Use Macie for sensitive data discovery
- Implement backup strategies across regions
- Use AWS Backup for centralized backup management

### Monitoring and Detection
- Enable CloudTrail for all regions
- Configure VPC Flow Logs
- Set up CloudWatch Logs and metrics
- Implement AWS Config rules
- Use Security Hub for compliance monitoring
- Set up GuardDuty for threat detection
- Implement automated remediation with EventBridge
- Use AWS X-Ray for application tracing

### Compute Security
- EC2 security groups with minimal inbound rules
- Regular patching of EC2 instances
- Use Systems Manager for patch management
- AMI hardening and validation
- Container image scanning
- Function/container immutability
- Implement appropriate instance termination protection
- Use Auto Scaling for reliability

## 4. AWS Trust Boundaries

- AWS Account boundary
- VPC boundary
- Subnet boundary
- Security group boundary
- Availability Zone boundary
- Region boundary
- Organizational unit boundary
- Landing zone boundary
- Public/private subnet boundary

## 5. Template Structure

When building your threat model template, organize components and stencils in this hierarchical structure:

1. **Categories** (Compute, Storage, Database, etc.)
2. **Services** (EC2, S3, RDS, etc.)
3. **Components** (Instances, Buckets, Databases, etc.)
4. **Connectors** (API calls, network traffic, data flow)
5. **Threats** (STRIDE categories)
6. **Mitigations** (Preventative controls)
7. **Trust Boundaries** (Account, VPC, etc.)

Each component should have associated threats and mitigations from the STRIDE model relevant to AWS services. 