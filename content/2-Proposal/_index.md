---
title: "Proposal"
date: 2025-09-09
weight: 2
chapter: false
pre: " <b> 2. </b> "
---


# ONLINE PLATFORM FOR TRACKING AND FORECASTING HURRICANE TRAJECTORY
## Geodesic-Aware Deep Learning for Hurricane Trajectory Prediction: A Physics-Informed and Augmentation-Driven Approach

### 1. Executive Summary

Time-series data serves as one of the most fundamental representations of information in modern scientific and industrial applications. It is essential for understanding dynamic processes such as economic trends, energy consumption patterns, and meteorological changes over time. In particular, weather forecasting heavily relies on time-series data to predict future atmospheric conditions, hurricane trajectories, and seasonal anomalies based on historical records.

With the rapid progress in deep learning and neural network research, our project aims to develop an advanced forecasting model capable of accurately predicting the future path, intensity, and total travel distance of moving storms within the next few days. The model’s predictions can support early warning systems, allowing authorities and residents in affected regions to take precautionary measures well before a hurricane reaches their area.

To move beyond the limitations of current technologies and existing research, this study introduces several novel techniques and algorithms, including two new augmentation methods for geodesic time-series data and a spatial encoding mechanism designed to enhance the predictive performance of convolutional computing. The final system will be integrated into a cloud-based, serverless architecture on AWS, ensuring scalability, high availability, and cost efficiency for real-time storm tracking and analysis.

The system architecture leverages several AWS services to form a fully managed data processing and deployment pipeline. AWS Lambda functions serve as the backbone for serverless computation, automatically triggered by Amazon EventBridge to crawl and process new storm data from open meteorological sources on a scheduled basis. Processed data is stored securely in Amazon S3, while AWS CodePipeline and CodeBuild automate the continuous integration and deployment of new model versions. The trained model is hosted and exposed via Amazon API Gateway, enabling lightweight, real-time inference requests from the online forecasting platform. All system activities are monitored through Amazon CloudWatch, providing operational visibility, fault detection, and performance metrics.

The first proposed method, Stepwise Temporal Fading Augmentation (STFA), is a new time-series augmentation framework that models the natural decline in the influence of past observations. Unlike traditional approaches based on random perturbation or noise injection, STFA applies fading weights to earlier time steps while preserving recent information. This process generates realistic and diverse synthetic sequences, improving model robustness and generalization. The technique will be evaluated on hurricane trajectory prediction tasks that rely on sequential latitude–longitude data.

The second proposed technique, Plausible Geodesic Bearing Augmentation (PGBA), introduces an augmentation strategy based on the feasible range of storm bearings and distances. By analyzing the geodesic bearing and distance between consecutive storm locations, PGBA defines a realistic motion boundary within which new synthetic trajectories are generated. This approach enhances the model’s capacity to capture natural spatial variability and directional uncertainty in storm movements.

Additionally, this study explores a spatial–temporal representation of time-series data, enabling the application of convolutional neural networks (CNNs) to capture both spatial and temporal dependencies. This representation leverages the strengths of convolutional computing to model local interactions across space and time. It will serve as a baseline for comparison with Temporal Convolutional Network (TCN) models trained using the proposed augmentation methods.

Traditional neural networks, whether used for sequential or image-based modeling, primarily learn statistical patterns from data. However, in many real-world physical systems, such purely data-driven models may fail to adhere to natural constraints, such as gravitational or geodesic relationships. To address this limitation, we incorporate the principles of Physics-Informed Machine Learning (PIML) into our approach. Specifically, geodesic distance and bearing are derived from the latitude–longitude data and integrated into the model’s training process as physically meaningful features. Furthermore, we employ the Haversine formula—which computes the spherical distance between two points—as an auxiliary loss term, complementing standard error metrics such as MSE, RMSE, MAE, and MAPE.

By combining the proposed augmentation methods, physics-informed learning principles, and a serverless deep learning infrastructure powered by AWS services, this research aims to develop a scalable, robust, and accurate framework for hurricane trajectory forecasting. The resulting system not only advances the state of geodesic time-series modeling but also demonstrates the practicality of deploying AI-driven environmental prediction systems as resilient, cloud-native applications that enhance preparedness and safety in hurricane-prone regions.

### 2. Problem Statement
### What’s the Problem?

To develop a reliable platform for tracking and issuing alerts on future hurricane movements, it is essential to construct a machine learning model that is both accurate and capable of producing dependable predictions. The tracking component can be addressed by continuously collecting and updating data from publicly available meteorological sources. However, these datasets are often geographically constrained and contain redundant or incomplete information.

In contrast, the predictive component presents greater complexity. Achieving accurate time-series forecasting in this context typically faces two primary challenges: (1) the limited diversity and coverage of available data, and (2) the absence of physical grounding, which restricts the model’s ability to reflect the underlying geophysical dynamics of hurricane behavior.

- **Data scarcity**: Many time-series forecasting tasks suffer from limited training data. While there are various augmentation methods, few approaches directly focus on the declining importance of past values over time.

- **Physics ignorance**: Most neural networks only learn from raw data, without considering real-world physical constraints. In trajectory prediction tasks (e.g., hurricanes), this often leads to unrealistic predictions.

We aim to:
+ Develop a new time-series augmentation method (STFA and PGBA) to improve robustness and generalization.
+ Incorporate physics-based constraints into model training, bridging the gap between data-driven learning and real-world dynamics.
+ Testing the strength of convolution network (convol2D) in term of forecasting trajectory
+ Creating an online platform that provide latest information about currents storm and precisely predictions on their trajectory

### The Solution

#### A - Stepwise Temporal Fading Augumentation

STFA generates synthetic time-series sequences by gradually reducing the influence of earlier values. Unlike random noise injection, it systematically applies stepwise fading multipliers across bands of older data.

Let a univariate sequence be:

$$
X = [x_0, x_1, \ldots, x_{T-1}]
$$

where $T$ is the sequence length of $X$.

**Parameters**:

- $n$: number of most steps to remain unchanged.  
- $S$: number of step-bands to apply fading, each band is assigned a constant multiplier.  
- $L = T - n$: length of the fading region.  
- $k = \frac{L}{S}$: values per band.  
- $I_b$: index set of the $b$-th band.  


\\[
I_b = \{\ {i \mid L - b \cdot k \;\leq\; i \;\leq\; L - (b-1)\cdot k - 1} \,\}
\\]

**Transformation**:

We denote the augmented series as:

$$
X = [x_0, \ldots, x_{T-1}]
$$


with the transformation rules:

$$
x_t =
\begin{cases}
x_t, & t \in \{T-n, \ldots, T-1\}, \\\
m_b \, x_t, & t \in I_b, \\\
m_{S+1} \, x_t, & t < \min(I_S),
\end{cases}
$$

where multipliers $m_b \in (0,1)$ decrease monotonically from recent to older bands.

This formulation preserves the fidelity of recent history while exerting stronger control on the long-range influence of the sequence. The augmentation forces the model to focus on robust patterns beyond the raw data, while increasing diversity according to the chosen parameters.

#### B - Plausible Geodesic Bearing Augmentation

The Plausible Geodesic Bearing Augmentation (PGBA) technique enhances the realism and control of synthetic trajectory generation in geospatial time-series forecasting tasks. Unlike conventional random perturbation methods, PGBA introduces stochasticity that remains plausible within the physical constraints of the underlying motion. The generated trajectories are derived from the geometric relationships of past observations rather than from purely random steps, resulting in smoother paths and meaningful variability in the training dataset. This technique apply on every four locations in a data sequence. 

PGBA serves as a complementary augmentation to STFA, enriching the diversity of training samples while preserving the original dynamical structure. Its objective is to create redundant yet physically consistent trajectories that capture potential variations in storm movements or similar geospatial phenomena.

 **Core Mechanism**

Consider a storm trajectory represented by a sequence of $n$ ordered geographical points:

$$
P = [P_1, P_2, \ldots, P_n]
$$

We split this sequence into small blocks of 4 points, with $P_i$ as the starting point of each block, defined by its latitude and longitude:

$$
P_i = (\phi_i, \lambda_i)
$$

Here, $\phi_i$ and $\lambda_i$ denote latitude and longitude in radians, respectively.

The geodesic distance $d_i$ and bearing $\theta_i$ between consecutive points $P_i$ and $P_{i+1}$ are defined as:

$$
d_i = \text{Distance}(P_i, P_{i+1}), \qquad
\theta_i = \text{Bearing}(P_i, P_{i+1})
$$

To introduce plausible variability, PGBA perturbs the bearing by adding a small, uniformly distributed random noise $\epsilon_i$:

$$
\theta_i^{\text{aug}} = \theta_i + \epsilon_i, \qquad
\epsilon_i \sim \text{Uniform}(-\delta, \delta)
$$

where $\delta$ is a tunable angular bound controlling the range of deviation.

The first two points always remain unchanged and are used to compute distance and bearing.
The subsequent augmented point is then computed using the geodesic destination formula, keeping the distance constant while allowing the bearing to vary within the range of the random noise:

$$
P_{i+2}^{\text{aug}} = \text{Destination}(P_i, d_i, \theta_i^{\text{aug}})
$$

This process preserves the inter-point distance $d_i$ while slightly perturbing the direction to produce physically plausible deviations.

 **Multi-Step Smoothing and Correction**
 
To enhance spatial smoothness and generate curvilinear trajectories, PGBA applies a secondary correction at every fourth point.
Let $P_{i+3}^{\text{aug}}$ denote the fourth point. It is recomputed such that its bearing $\theta_{i+3}^{\text{corr}}$ minimizes the deviation from the original point $P_{i+3}$:

$$
\theta_{i+3}^{\text{corr}} =
\arg\min_{\theta},
\text{Distance}\Big(
\text{Destination}(P_{i+2}^{\text{aug}}, d_{i+2}, \theta),;
P_{i+3}
\Big)
$$

Then, the corrected augmented point is obtained as:

$$
P_{i+3}^{\text{aug}} =
\text{Destination}(P_{i+2}^{\text{aug}}, d_{i+2}, \theta_{i+3}^{\text{corr}})
$$

This step ensures smooth transitions across multiple points while maintaining physical plausibility.

Note that the first two and last locations in every geodesic time-series sequence always remain unchanged.

## C - Physics-Informed Machine Learning

Neural network models such as RNNs, CNNs, and Transformers do not require explicit formulas or task-specific rules to perform well, provided that they are trained with sufficient data. For example, in machine translation tasks such as German-to-English translation using an RNN, no explicit grammar rules are provided during training. Nevertheless, the model is capable of producing coherent translations, which demonstrates one of the major strengths of deep learning: the ability to learn complex patterns directly from data. In contrast, traditional approaches—such as early versions of rule-based translation systems (e.g., Google Translate prior to the 2000s)—relied heavily on grammar rules and dictionaries. While precise, such systems often lacked flexibility and failed when encountering words with multiple meanings or when handling context-dependent structures.

Inspired by this, our goal is to combine the strengths of deep learning with human-defined formulas in order to achieve better performance. Specificly in this geography field,  we will try to add the benefit from Haversine formula into training for distance and beering calculation between two location on a sphere. These provide the model with additional structure and inductive bias, guiding learning beyond purely statistical correlations.

**Haversine Formula**

*For distance calculation*

The Haversine formula is used to calculate the great-circle distance between two points on the surface of a sphere — that is, the shortest path over the Earth’s surface.

$$
d = 2r \, \arcsin\!\left( 
  \sqrt{ 
    \sin^2\!\left(\frac{\Delta \varphi}{2}\right)
    + \cos(\varphi_1)\cos(\varphi_2)
      \sin^2\!\left(\frac{\Delta \lambda}{2}\right)
  } 
\right)
$$

Where:
- $\varphi_1, \lambda_1$ and $\varphi_2, \lambda_2$ are the latitudes and longitudes of the two points (in radians).  
- $\Delta \varphi = \varphi_2 - \varphi_1$  
- $\Delta \lambda = \lambda_2 - \lambda_1$  
- $r$ is the Earth’s radius (≈ 6,371 km).  

In our framework, instead of relying solely on standard loss functions such as MSE, RMSE, or MAPE, we propose using the Haversine formula for distance calculation as the primary loss function. As the model outputs latitude and longitude coordinates for the next hurricane location, the Haversine formula directly measures the distance between predicted and ground-truth points. A distance close to 0 indicates a highly accurate prediction, while a large distance signals a significant error.  

*For beering calculation*

The Bearing calculation is dereived from Haversine Formula gives the direction from one geographic point to another along the great-circle path:

$$\theta = \text{atan2}\!\left(\sin(\Delta \lambda)\cos(\varphi_2),\, \cos(\varphi_1)\sin(\varphi_2) - \sin(\varphi_1)\cos(\varphi_2)\cos(\Delta \lambda)\right)$$

Where:
- $(\varphi_1, \lambda_1)$ is the start point.  
- $(\varphi_2, \lambda_2)$ is the end point.  
- $\Delta \lambda$ is the difference in longitude.  

The result $\theta$ represents the initial bearing (azimuth) measured clockwise from true north.

In our implementation, we fully exploit the use of Haversine Formula to compute two additional features — “distance” and “bearing” — which are appended to the dataset.  
These features provide the model with richer information about hurricane trajectories while maintaining the core objective of predicting the next geographic location.

## D - Short overview of misconceptions in Common Approaches to Sequence Modeling

In the field of sequence modeling within deep learning, recurrent architectures such as RNNs, LSTMs, and GRUs are often considered the default solutions. This perception has led many practitioners and researchers to overlook alternative architectures, particularly convolutional neural networks (CNNs), which are traditionally associated with image processing tasks. Textbooks and courses frequently categorize tasks such as language modeling, translation, or other sequential predictions as the domain of recurrent networks, while convolutional networks are presented primarily in the context of spatial data like images. As a result, the potential of CNNs for sequence modeling is frequently underestimated or ignored.

**Convolutional networks** offer several intrinsic advantages that make them well-suited for sequential data. Their inherent parallelism allows for significantly faster training compared to strictly sequential models. Additionally, CNNs are highly effective at capturing local spatial and temporal dependencies, a property that can be leveraged in time-series forecasting and other sequential tasks. Despite these benefits, CNNs are often misunderstood as being unsuitable for sequences due to their lack of explicit memory mechanisms and the absence of intrinsic temporal ordering.

In our study, we focus on hurricane trajectory prediction, where the data consists of time-series records of latitude and longitude coordinates. We demonstrate that this type of sequential data can be effectively modeled using CNNs, leveraging their computational efficiency and ability to capture local spatiotemporal patterns. To facilitate this, we encode the locations into a 2D matrix representation, enabling the convolutional network to more effectively extract and learn patterns from the data. Each entry in the matrix corresponds to a “pixel” of an image, an approach we refer to as **Trajectory-as-Image**. Our methodology employs a standard CNN as a baseline, which is then compared with more specialized sequence models, including TCNs, LSTMs, and RNNs, while incorporating various data augmentation techniques, including two novel methods proposed in this work.

Through systematic experimentation and evaluation, we aim to challenge the prevailing notion that convolutional architectures are ill-suited for sequential data. By highlighting the effectiveness of CNNs in sequence modeling, we hope to broaden the perspective of researchers and practitioners, encouraging them to explore convolutional computing as a viable and competitive approach in time-series forecasting and other sequential prediction tasks.

### Benefits and Return on Investment

- **Performance Boost**: STFA + PGBA generates structured synthetic sequences that enhance model robustness, reduce overfitting, and improve generalization on unseen storm trajectories.

- **Physics Awareness**: Incorporating geographical principles such as distance and bearing increases interpretability and ensures physically consistent predictions.

- **New Research Direction**: Establishes two novel paradigms for time-series augmentation based on temporal relevance fading, expanding the methodological toolkit for sequence learning.

- **Scalability and Reusability**: The combined STFA + PGBA + PIML framework can be extended to other sequential forecasting domains such as energy demand, traffic flow, and financial trends.

**Overall Impact**: By improving predictive stability and interpretability while maintaining scalability, the proposed approach delivers both scientific value and practical return on computational investment.

### 3. Solution Architecture

The online platform provides users with up-to-date information on recent storms and a powerful tool for hurricane trajectory predictions. Visitors can either view recent storm data or run predictions using the ML models. The results are displayed interactively on a map, showing storm location, time, and predicted path.

The platform is built using a serverless AWS architecture to reduce operational costs while maintaining scalability and reliability. Frontend content is hosted on Amazon S3 and delivered globally via CloudFront, ensuring low-latency access. Users’ requests are routed through API Gateway to Lambda functions, which handle prediction computations and data retrieval. Pre-trained ML models and recent storm datasets are securely stored in S3, with weekly updates managed automatically by EventBridge-triggered crawler Lambdas. Sensitive API keys are stored in Secrets Manager, and system performance is monitored through CloudWatch logs and metrics. IAM enforces least-privilege access for all services.

The predictive models leverage the proposed STFA (Stepwise Temporal Fading Augmentation) and PGBA (Plausible Geodesic Bearing Augmentation) techniques. These methods generate realistic synthetic time-series trajectories, preserving temporal relevance and spatial consistency, which significantly improves the robustness and accuracy of the models. By integrating STFA and PGBA, the platform delivers more precise hurricane path predictions, helping users better understand storm behavior and make informed decisions.

This architecture enables a responsive, cost-efficient, and secure platform where users can visualize real-time storm information and explore hurricane trajectory forecasts with interactive maps, supported by advanced augmentation methods to boost model performance.

<p align="center">
  <img src="/images/2-Proposal/trainning_process.png" alt="Picture 1" />
  <br/>
  <strong style="font-size: 18px;">Figure 1 : Model pipeline</strong>
</p>

<p align="center">
  <img src="/images/2-Proposal/skynet_diagram.png" alt="Picture 2" />
  <br/>
  <strong style="font-size: 18px;">Figure 2 : Platform Architecture</strong>
</p>


### AWS Services Used

* **Amazon S3**: Stores static frontend files, pre-trained ML models, and recent storm data.
* **AWS Lambda**: Runs prediction models, fetches storm data, and executes web crawling automation.
* **Amazon API Gateway**: Handles frontend requests for predictions and storm data.
* **Amazon CloudFront**: Delivers static content globally with low latency.
* **Amazon Route 53**: Routes user traffic to CloudFront.
* **Amazon EventBridge**: Schedules weekly data crawling.
* **AWS Secrets Manager**: Stores external API keys securely.
* **Amazon CloudWatch**: Monitors Lambda logs, performance metrics, and system health.
* **AWS IAM**: Assigns least-privilege access to all services.

### Component Design

* **Frontend Layer**: Hosted on **S3** and delivered through **CloudFront**.
* **Backend Layer**: Handles prediction and data retrieval using **API Gateway** and **Lambda**.
* **Data Storage**: ML models stored in S3, recent storm data updated weekly by the crawler.
* **Automation**: **EventBridge** triggers a weekly **Crawler Lambda** to fetch external storm data.
* **Security & Monitoring**: Secrets stored in **Secrets Manager**, metrics and logs collected via **CloudWatch**.

### 4. Technical Implementation

**Implementation Phases**
This project has three main parts: building the prediction pipeline, setting up data crawling, and deploying the web platform. Each part follows four phases:

* **Design Architecture**: Plan AWS serverless stack, Lambda functions, S3 structure. (Weak 1)
* **Estimate Costs**: Use AWS Pricing Calculator to assess feasibility and adjust design (Weak 1-2).
* **Optimize Architecture**: Fine-tune Lambda memory, S3 usage, and caching to reduce cost (Weak 2-4).
* **Develop, Test, Deploy**: Implement Lambda functions, event scheduling, ML model integration, and web frontend with Next.js (Weak
4-8).

**Technical Requirements**

* **ML Models**: Pre-trained trajectory models stored in S3 (.h5/.pth), loaded by prediction Lambda.
* **Storm Data**: Weekly updated JSON files, stored in S3, used for frontend display and prediction validation.
* **Serverless Infrastructure**: Lambda for prediction, fetching, and crawling; API Gateway for frontend requests; CloudFront/S3 for content delivery.
* **Security**: Secrets Manager for API keys, IAM for least-privilege access.
* **Monitoring**: CloudWatch for logging and Lambda Insights metrics.

### 5. Timeline & Milestones

**Project Timeline**

* **Pre-Internship (Weak 1)**: Planning, research on external weather APIs, and ML model preparation.
* **Internship (Weak 1-8)**:

  * Weak 1-2: AWS study, architecture design, and cost estimation.
  * Weak 2-4: Optimize architecture, configure serverless workflow, and integrate ML models.
  * Weak 4-8: Implement Lambda functions, set up frontend, test system, and deploy to production.
* **Post-Launch**: Continuous data collection and monitoring for up to 1 year.

### 6. Budget Estimation

**Region:** ap-southeast-1 (Singapore)

The estimated monthly costs for running the hurricane prediction platform on AWS are as follows:

**A. Frontend & Content Delivery**

* **Amazon S3 (Static Files):** Hosts 5 GB of frontend files (HTML, CSS, JS) and handles 10 GB of data transfer per month. Cost ≈ **$0.54/month**.
* **Amazon CloudFront:** Handles 50 GB of data transfer and up to 1 million requests (within free tier). Cost ≈ **$6.00/month**.
* **Amazon Route 53:** 1 hosted zone and 1 million DNS queries per month. Cost ≈ **$0.90/month**.
* **AWS Certificate Manager (ACM):** Provides TLS certificate for secure HTTPS access. Free of charge.

**Subtotal for Frontend & CDN:** ≈ **$7.4/month**

**B. Backend (API & ML Processing)**

* **Amazon API Gateway:** Handles 1 million HTTP API requests per month, each request approximately 1 MB in size. Cost ≈ **$2.5/month**.
* **Lambda (Storm Prediction):** Predicts hurricane trajectories using ML models. Runs ~1,000 times per day with 512 MB memory allocated and 1 GB ephemeral storage. Each execution lasts ~5 seconds. Cost ≈ **$2.54/month**.
* **Lambda (Fetch Recent Storm Data):** Fetches storm data from S3 for the frontend. Runs ~20,000 times per day with 512 MB memory and 512 MB ephemeral storage for 1 second per execution. Cost ≈ **$0.00/month** (covered by free tier).

**Subtotal for Backend:** ≈ **$4.54/month**

**C. Automation & Data Crawling**

* **Amazon EventBridge:** Schedules weekly crawling of storm data (1 cron trigger per day). Free under AWS free tier.
* **Lambda (Web Crawler):** Fetches data from external APIs weekly. Uses 128 MB memory and 512 MB ephemeral storage, ~30 seconds per execution. Cost ≈ **$0.00/month** (free tier).
* **AWS Secrets Manager:** Stores 5 API keys for secure access to external weather services. Cost ≈ **$2.00/month**.

**Subtotal for Automation & Data Crawling:** ≈ **$2.0/month**

**D. Monitoring & Logging**

* **Amazon CloudWatch Logs:** Collects logs from all Lambda functions and delivers to S3 with 1-month retention. Approximately 2 GB of logs per month. Cost ≈ **$0.57/month**.
* **CloudWatch Metrics (Lambda Insights):** Monitors 8 metrics across Lambda functions. First 10 metrics are free, and only record for predictions and crawl data. Cost ≈ **$0.00/month**.

**Subtotal for Monitoring & Logging:** ≈ **$0.57/month**

**E. Storage & Data Transfer**

* **S3 (Model Bucket):** Stores ML models (~1 GB) and handles ~60,000 GET requests per month. Cost ≈ **$0.05/month**.
* **S3 (Recent Storms Bucket):** Stores recent storm data (~1 GB) with ~60,000 GET requests and 30 PUT requests per month. Cost ≈ **$0.27/month**.

**F. Tooling & Migration**

* AWS CodePipeline and CodeBuild for 10 minutes fixing ≈ $0.90/month.

**Subtotal for Storage & Data Transfer:** ≈ **$0.32/month**

**Total Estimated Monthly Cost**

* Frontend & CDN: $7.4
* Backend (API + ML): $4.54
* Automation (Crawler + Secrets): $2.0
* Monitoring & Logging: $0.57
* Storage & Data Transfer: $0.32
* Tooling & Migration: $0.9

**TOTAL ≈ $15.73/month**

### 7. Risk Assessment

**Risk Matrix**

* **Network Outages**: Medium impact, medium probability.
* **Data Source Unavailability**: Medium impact, low probability.
* **ML Model Errors**: High impact, low probability.
* **Cost Overruns**: Medium impact, low probability.

**Mitigation Strategies**

* **Network**: Cache recent storm data in S3 to allow frontend display during outages.
* **Data Sources**: Store historical storm data for fallback.
* **ML Model**: Regular model validation and testing.
* **Cost**: Monitor AWS usage and set budget alerts.

**Contingency Plans**

* Switch to manual updates if external API fails.
* Rollback to previous ML model using S3 versioning if new model fails.

### 8. Expected Outcomes

**Technical Improvements:**

* Real-time hurricane trajectory predictions with visualized paths.
* Scalable serverless system capable of handling thousands of requests/day.

**Long-term Value:**

* Centralized hurricane data for research and analysis.
* Framework reusable for other geospatial prediction tasks.
* Low monthly operational cost (< $20/month).