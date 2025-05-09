## 2. Project Instructions

### Objective

Create a minimum viable product (MVP) within 3-4 weeks that demonstrates the core value proposition, focusing on web-based access and basic mobile audio recording capability.

### Development Approach

#### MVP Tech Stack

- **Frontend**: Next.js with TypeScript, Tailwind CSS, shadcn/ui components
- **Backend**: Python FastAPI for AI/ML components, Next.js API routes for UI operations
- **Authentication**: Supabase Auth with legal firm account management
- **Database**: Supabase PostgreSQL with legal document metadata schema
- **Storage**: S3-compatible storage for documents and audio recordings
- **Mobile App**: React Native for iOS/Android with audio recording capabilities
- **AI Components**:
  - Vector Store: Pinecone (serverless)
  - LLM Integration: OpenAI + LangChain (Python)
  - Speech-to-Text: OpenAI Whisper API
  - Document Processing: PyPDF2/pdfminer, python-docx, langchain.document_loaders
- **Legal Integration**:
  - South African case law API integration
  - Legal citation parser and validator
  - Legal document template engine
- **Hosting**:
  - Vercel (web frontend)
  - Railway or Fly.io (Python FastAPI backend)
  - Supabase (authentication/database)
  - AWS S3 or equivalent (document/audio storage)

#### Development Principles

1. **Legal-First Design**: Optimize for legal workflows and South African legal context
2. **Rapid Prototyping**: Focus on functional core features first, especially legal search
3. **Daily Progress**: Each day should produce visible progress on key features
4. **User-Centered Design**: Build with legal professionals as primary users
5. **South African Context**: Ensure relevance to SA legal framework and terminology
6. **Documentation First**: Document architecture decisions as they're made

### MVP Core Features (Detailed)

1. **Legal Document Intelligence System**

   - Document upload with legal document type classification
   - Legal text extraction with citation recognition
   - South African legal framework knowledge integration
   - Vector embedding generation optimized for legal terminology
   - Semantic legal search with precedent matching
   - Legal document preview with citation highlighting

2. **Legal Database Integration**

   - Connection to established South African legal databases
   - Case law retrieval and citation
   - Legal article and source integration
   - Credibility scoring for legal sources
   - Search across multiple legal repositories
   - Export of source lists with proper legal citation format

3. **Audio Recording & Processing**

   - Mobile app for recording legal proceedings
   - Secure audio storage and transmission
   - Speech-to-text transcription optimized for legal terminology
   - Automatic speaker identification
   - Legal document generation from transcripts
   - Rich-text format export (compatible with MS Word, Google Docs)

4. **Natural Language Legal Interface**

   - Legal query processing with South African context
   - Case law and statute reference recognition
   - Context-aware legal responses with jurisdiction handling
   - Source citation with verification links
   - Conversation history with legal topic categorization
   - Query refinement suggestions for legal clarity

5. **User & Content Management**
   - Legal firm account structure
   - Role-based access (attorney, paralegal, staff)
   - HR department specific permissions
   - Document access controls with client-matter attribution
   - Usage tracking for billing purposes
   - Activity logs for legal compliance
