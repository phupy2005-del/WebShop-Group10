```mermaid
graph TD
    subgraph User_Input [Input Layer]
        A["User Query - Tiếng Việt"]
    end

    subgraph Query_Processing [1. Query Processing]
        B["Translation - Google/Helsinki"] --> C{Query Strategy}
        C -->|Baseline| D["CLIP Tokenizer"]
        C -->|Expansion| E["Query Expansion/LLM Rewrite"]
        E --> D
        D --> F["CLIP Text Encoder - ViT-B/32"]
        F --> G["Query Vector - 512-d"]
    end

    subgraph Vector_DB [2. Vector Search - ANN]
        G --> H["Vector Database - Qdrant/FAISS"]
        H --> I{Search Algorithm}
        I -->|Flat| J["Exact Search"]
        I -->|IVF| K["Inverted File Index"]
        I -->|HNSW| L["Hierarchical Navigable Small World"]
        I -->|IVFPQ| M["Product Quantization"]
    end

    subgraph Post_Processing [3. Refining Results]
        J & K & L & M --> N["Candidate Vectors"]
        N --> O["Re-ranking - Cross-encoder/LLM"]
        O --> P["Metadata Filter - Video/Timestamp"]
    end

    subgraph Output_Layer [Output]
        P --> Q["Top-K Keyframes"]
    end

    style G fill:#f96,stroke:#333,stroke-width:2px
    style Q fill:#bbf,stroke:#333,stroke-width:2px
