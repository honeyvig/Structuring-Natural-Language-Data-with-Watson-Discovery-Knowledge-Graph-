# Structuring-Natural-Language-Data-with-Watson-Discovery-Knowledge-Graph
To structure and analyze natural language data using IBM Watson Discovery with its Knowledge Graph capabilities, you can follow these steps. Watson Discovery allows you to extract insights from unstructured data, and the Knowledge Graph enables you to create a semantic representation of your data, including entities, relationships, and concepts.

The process involves:

    Preparing your natural language data (text).
    Using Watson Discovery to analyze the data.
    Building a Knowledge Graph for structured understanding.

Step 1: Set Up IBM Watson Discovery

First, you need to set up IBM Watson Discovery. You'll need an IBM Cloud account, and then you can create a Watson Discovery service instance.

Here’s the link to get started:
IBM Watson Discovery

Once your service is up and running, you’ll need:

    The API Key and URL to interact with Watson Discovery via API.
    A project to store the data you're going to upload and analyze.

Step 2: Install Required Libraries

To interact with Watson Discovery, you’ll need the ibm-watson Python SDK. You can install it via pip:

pip install ibm-watson

Step 3: Set Up Watson Discovery Client

Before you start, you’ll need to authenticate and initialize the Watson Discovery client using the credentials you received when setting up your service.

from ibm_watson import DiscoveryV2
from ibm_cloud_sdk_core.authenticators import IAMAuthenticator

# Set up Watson Discovery API credentials
api_key = 'YOUR_API_KEY'
url = 'YOUR_SERVICE_URL'
version = '2021-08-01'  # Make sure to use the latest API version

# Authenticate and create a Discovery instance
authenticator = IAMAuthenticator(api_key)
discovery = DiscoveryV2(version=version, authenticator=authenticator)
discovery.set_service_url(url)

Step 4: Upload Natural Language Data

You need to upload documents (in formats such as PDFs, HTML, or text files) into Watson Discovery so that it can process and analyze them. You can do this by creating a collection and uploading documents to it.

# Create a new collection in the Watson Discovery instance
environment_id = 'YOUR_ENVIRONMENT_ID'  # Find this in your Watson Discovery dashboard
collection_name = 'my-collection'

# Create the collection
create_response = discovery.create_collection(
    environment_id=environment_id,
    name=collection_name
).get_result()

print(create_response)

Now, you can upload documents to the collection.

# Upload documents (using text files as an example)
document_file = 'path_to_your_file.txt'  # Path to your file
with open(document_file, 'r') as file:
    file_content = file.read()

# Add document to collection
add_document_response = discovery.add_document(
    environment_id=environment_id,
    collection_id=create_response['collection_id'],
    file=file_content
).get_result()

print(add_document_response)

Step 5: Analyze the Documents with Watson Discovery

Once the documents are uploaded, Watson Discovery will analyze them to extract insights, including entities, keywords, and concepts. You can use its query feature to retrieve meaningful insights.

# Query the collection to retrieve structured insights
query_response = discovery.query(
    environment_id=environment_id,
    collection_id=create_response['collection_id'],
    query='*',  # Use "*" to retrieve all the insights
    natural_language_query="Search for entities",  # Your query
    count=5  # Adjust based on the number of results you want
).get_result()

print(query_response)

Step 6: Use Watson Discovery Knowledge Graph

Now that you have extracted the data, you can use Knowledge Graph capabilities to structure and understand relationships between entities.

To build a Knowledge Graph, you'll need to enable the Entities and Relations feature in Watson Discovery.
Example: Use the Entity and Relation extraction:

Here’s how you can structure and view relationships between entities using Watson Discovery’s Knowledge Graph API.

# Query with entity extraction enabled
entity_query_response = discovery.query(
    environment_id=environment_id,
    collection_id=create_response['collection_id'],
    query='*',  # Wildcard query for all documents
    natural_language_query="Identify relations between entities",
    highlight=True,
    return_fields=['entities', 'relations'],
    count=5
).get_result()

print(entity_query_response)

In the output, you’ll see the extracted entities (people, places, things) and relations (connections between them) in the documents.
Step 7: Visualize the Knowledge Graph

You can use the extracted entities and relations to visualize the Knowledge Graph. Watson Discovery itself doesn’t provide a direct visualization tool, but you can use libraries like NetworkX (for graph visualization) in Python to visualize the relationships.

import networkx as nx
import matplotlib.pyplot as plt

# Sample: Creating a graph from the extracted entities and relations
G = nx.Graph()

# Add nodes (entities)
for entity in entity_query_response['entities']:
    G.add_node(entity['text'], type=entity['type'])

# Add edges (relations)
for relation in entity_query_response['relations']:
    G.add_edge(relation['entity1']['text'], relation['entity2']['text'])

# Visualize the graph
nx.draw(G, with_labels=True, font_weight='bold', node_size=3000, node_color='skyblue')
plt.show()

Step 8: Analyze and Explore Further

Now, with your data processed and structured into a Knowledge Graph, you can perform various analyses:

    Entity Linking: Track how entities are linked across documents.
    Sentiment Analysis: Combine Watson Discovery's natural language processing capabilities with sentiment analysis to gain insights into how entities relate emotionally in the data.

You can expand your analysis further depending on the specific questions you want to answer from the natural language data.
Conclusion

You now have a basic setup for working with IBM Watson Discovery and creating a Knowledge Graph from your natural language data. The above steps show how to prepare, upload, and analyze documents, then structure the insights using Watson's built-in NLP tools, and finally visualize relationships between extracted entities.
