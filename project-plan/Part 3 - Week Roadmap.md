## 3. 4-Week Roadmap with Progress Tracking

### Week 1: Foundation & Setup (May 7-13, 2025)

#### Detailed Task Checklist

##### Project Repository

- [ ] Create GitHub organization "Verdict360-Legal"
- [ ] Initialize repositories (web, api, mobile)
- [ ] Set up branch protection rules with legal compliance emphasis
- [ ] Configure GitHub project board with legal workflow labels
- [ ] Create issue templates for legal document features
- [ ] Set up CI/CD workflow files (web, api, mobile)

##### Next.js Setup & Legal UI Framework

- [ ] Initialize Next.js project with TypeScript
- [ ] Install and configure Tailwind CSS with legal color scheme
- [ ] Add shadcn/ui components with legal-themed customizations
- [ ] Set up ESLint and Prettier with team standards
- [ ] Configure TypeScript paths and aliases
- [ ] Add legal-specific folder structure (legal-docs, case-law, audio)

##### Mobile App Foundation (React Native)

- [ ] Initialize React Native project with TypeScript
- [ ] Configure native audio recording capabilities
- [ ] Set up secure file storage mechanisms
- [ ] Create basic UI components for recording
- [ ] Implement audio compression for efficient upload
- [ ] Add authentication flow with Supabase

##### Supabase Project & Legal Schema

- [ ] Create Supabase project with proper security settings
- [ ] Set up authentication with legal firm organizational structure
- [ ] Configure storage buckets for legal documents and audio
- [ ] Set up Row Level Security policies with client-matter confidentiality
- [ ] Generate and securely store API keys
- [ ] Test connection from local and mobile environments

##### Database Legal Schema

- [ ] Create legal users table with role specifications
- [ ] Implement legal documents table with jurisdiction fields
- [ ] Set up legal matters table for organization
- [ ] Create recordings table with transcription status
- [ ] Add case law reference tables
- [ ] Implement foreign key relationships with security in mind

##### Auth Flow with Legal Firm Structure

- [ ] Create login page with legal firm branding options
- [ ] Build registration form with firm/practice verification
- [ ] Implement firm administrator invitation flow
- [ ] Create protected routes with matter-level access control
- [ ] Set up auth context provider with legal roles
- [ ] Add user profile page with legal credentials

##### Basic Legal UI Layout

- [ ] Design navigation with legal workflow organization
- [ ] Create responsive header with practice switcher
- [ ] Build sidebar with legal document categorization
- [ ] Implement dark/light mode toggle with legal-themed colors
- [ ] Create legal-focused landing page
- [ ] Add footer with legal compliance information

| Task                         | Owner | Status      | Due    |
| ---------------------------- | ----- | ----------- | ------ |
| Project Repository Setup     |       | Not Started | May 7  |
| Next.js & Legal UI Setup     |       | Not Started | May 8  |
| Mobile App Foundation        |       | Not Started | May 8  |
| Supabase Legal Project Setup |       | Not Started | May 9  |
| Legal Database Schema        |       | Not Started | May 9  |
| Legal Auth Flow              |       | Not Started | May 10 |
| Legal UI Layout              |       | Not Started | May 13 |

**Week 1 Milestone**: Working application shell with legal firm authentication, navigation, and basic mobile recording capability

#### Week 1 Technical Implementation Details

```typescript
// Legal Database Schema (Supabase)
// legal_documents.sql
CREATE TABLE legal_documents (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  title TEXT NOT NULL,
  description TEXT,
  document_type TEXT NOT NULL, -- contract, judgment, statute, etc.
  jurisdiction TEXT NOT NULL DEFAULT 'South Africa',
  matter_id UUID REFERENCES legal_matters(id),
  file_url TEXT NOT NULL,
  file_type TEXT NOT NULL,
  created_by UUID REFERENCES auth.users(id),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  confidentiality_level TEXT NOT NULL DEFAULT 'standard'
);

// legal_matters.sql
CREATE TABLE legal_matters (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  title TEXT NOT NULL,
  reference_number TEXT,
  client_id UUID REFERENCES legal_clients(id),
  practice_area TEXT,
  responsible_attorney UUID REFERENCES auth.users(id),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  status TEXT NOT NULL DEFAULT 'active'
);

// legal_recordings.sql
CREATE TABLE legal_recordings (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  title TEXT NOT NULL,
  description TEXT,
  matter_id UUID REFERENCES legal_matters(id),
  audio_url TEXT NOT NULL,
  duration_seconds INTEGER,
  transcription_status TEXT DEFAULT 'pending', -- pending, in_progress, completed
  transcription_url TEXT,
  created_by UUID REFERENCES auth.users(id),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  recording_date TIMESTAMP WITH TIME ZONE
);
```

### Week 2: Legal Document & Audio Processing (May 14-20, 2025)

#### Detailed Task Checklist

##### Legal Document Upload

- [ ] Create drag & drop interface for legal documents
- [ ] Implement legal document type validation (statutes, judgments, contracts)
- [ ] Add progress indicators for uploads with size estimation
- [ ] Set up direct upload to Supabase storage with client-matter tagging
- [ ] Handle multiple file uploads with batch processing
- [ ] Add legal document validation and error handling

##### Audio Recording Integration (Mobile)

- [ ] Implement secure audio recording in React Native
- [ ] Create recording interface with legal proceeding metadata
- [ ] Add pause/resume/stop functionality
- [ ] Implement background recording capability
- [ ] Create audio review before upload feature
- [ ] Add automatic upload to secure storage

##### Python FastAPI Backend Setup

- [ ] Set up Python virtual environment with legal NLP packages
- [ ] Initialize FastAPI project with legal processing endpoints
- [ ] Create document and audio processing routes
- [ ] Set up Supabase client for Python with secure access
- [ ] Implement JWT validation for legal data security
- [ ] Create Docker configuration for deployment

##### Legal Text Extraction & Citation Detection

- [ ] Implement PDF text extraction optimized for legal documents
- [ ] Add DOCX parsing with legal formatting preservation
- [ ] Create TXT file processing with legal structure detection
- [ ] Implement legal citation recognition with regex patterns
- [ ] Add South African case law reference detection
- [ ] Set up extraction job queue with priority processing

##### Audio Transcription Integration

- [ ] Set up OpenAI Whisper API integration
- [ ] Create audio preprocessing pipeline
- [ ] Implement legal terminology optimization
- [ ] Add speaker identification for hearings/meetings
- [ ] Create legal transcript formatting
- [ ] Implement asynchronous processing with status updates

##### Vector Processing for Legal Content

- [ ] Set up OpenAI API integration with legal context
- [ ] Implement specialized legal document chunking logic
- [ ] Create embedding generation optimized for legal terminology
- [ ] Add South African legal context metadata
- [ ] Implement legal classification tagging
- [ ] Configure vector storage with legal access controls

##### Legal Database Integration

- [ ] Research and select South African legal database APIs
- [ ] Implement authentication and access methods
- [ ] Create search adapters for case law retrieval
- [ ] Add citation validation against legal databases
- [ ] Implement response formatting with proper legal citation
- [ ] Create cache layer for frequent legal references

| Task                              | Owner | Status      | Due    |
| --------------------------------- | ----- | ----------- | ------ |
| Legal Document Upload             |       | Not Started | May 14 |
| Audio Recording Integration       |       | Not Started | May 15 |
| Python FastAPI Backend            |       | Not Started | May 16 |
| Legal Text Extraction & Citations |       | Not Started | May 17 |
| Audio Transcription Integration   |       | Not Started | May 18 |
| Vector Processing for Legal Docs  |       | Not Started | May 19 |
| Legal Database Integration        |       | Not Started | May 20 |

**Week 2 Milestone**: Functional legal document processing pipeline and audio recording/transcription capability

#### Week 2 Technical Implementation Details

```python
# Legal Document Processing Pipeline (simplified)
# app/services/legal_document_processor.py
import os
from typing import List, Dict, Any
from langchain.document_loaders import PyPDFLoader, Docx2txtLoader, TextLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.embeddings.openai import OpenAIEmbeddings
from langchain.vectorstores import Pinecone
import pinecone
import re

# South African case citation regex patterns
SA_CASE_CITATION_PATTERNS = [
    r'\d{4}\s+\(\d+\)\s+SA\s+\d+\s+\([A-Z]+\)',  # 2019 (2) SA 343 (SCA)
    r'\[\d{4}\]\s+ZACC\s+\d+',                   # [2021] ZACC 13
    r'\[\d{4}\]\s+ZASCA\s+\d+',                  # [2020] ZASCA 99
    r'\d{4}\s+\(\d+\)\s+BCLR\s+\d+',             # 2018 (7) BCLR 844
    # Add more South African citation patterns as needed
]

# Legal document loader factory
def load_legal_document(file_path: str):
    """Load document based on file type with legal metadata extraction"""
    file_extension = os.path.splitext(file_path)[1].lower()

    if file_extension == '.pdf':
        return PyPDFLoader(file_path).load()
    elif file_extension in ['.docx', '.doc']:
        return Docx2txtLoader(file_path).load()
    elif file_extension == '.txt':
        return TextLoader(file_path).load()
    else:
        raise ValueError(f"Unsupported file type: {file_extension}")

# Legal citation detection
def extract_legal_citations(text: str) -> List[str]:
    """Extract South African legal citations from text"""
    all_citations = []

    for pattern in SA_CASE_CITATION_PATTERNS:
        citations = re.findall(pattern, text)
        all_citations.extend(citations)

    return all_citations

# Legal document chunking function
def chunk_legal_documents(documents, chunk_size: int = 1000, chunk_overlap: int = 200):
    """Split legal documents into chunks preserving legal context"""
    text_splitter = RecursiveCharacterTextSplitter(
        chunk_size=chunk_size,
        chunk_overlap=chunk_overlap,
        separators=["\n\n", "\n", ". ", " ", ""]
    )
    chunks = text_splitter.split_documents(documents)

    # Add citation metadata to chunks
    for chunk in chunks:
        chunk.metadata["citations"] = extract_legal_citations(chunk.page_content)

    return chunks

# Legal document embedding function
def generate_legal_embeddings(chunks: List[Any], document_id: str, jurisdiction: str = "South Africa"):
    """Generate embeddings optimized for legal content"""
    # Initialize Pinecone
    pinecone.init(
        api_key=os.environ.get("PINECONE_API_KEY"),
        environment=os.environ.get("PINECONE_ENVIRONMENT")
    )
    index_name = os.environ.get("PINECONE_INDEX")

    # Generate embeddings with legal context
    embeddings = OpenAIEmbeddings()

    # Create metadata for each chunk with legal context
    texts = [chunk.page_content for chunk in chunks]
    metadatas = [
        {
            **chunk.metadata,
            "chunk": i,
            "document_id": document_id,
            "jurisdiction": jurisdiction,
            "citations": chunk.metadata.get("citations", []),
            "legal_domain": determine_legal_domain(chunk.page_content)
        }
        for i, chunk in enumerate(chunks)
    ]

    # Store in Pinecone with legal metadata
    docsearch = Pinecone.from_texts(
        texts=texts,
        embedding=embeddings,
        metadatas=metadatas,
        index_name=index_name
    )

    return docsearch

# Helper function to determine legal domain
def determine_legal_domain(text: str) -> str:
    """Determine the legal domain of text (simplified - would use NLP classification)"""
    # This would be more sophisticated in production, using a classifier
    domains = {
        "constitutional": ["constitution", "constitutional", "bill of rights", "fundamental rights"],
        "criminal": ["criminal", "offence", "sentencing", "accused", "prosecution"],
        "civil": ["damages", "delict", "contract", "plaintiff", "defendant"],
        "corporate": ["company", "director", "shareholder", "business", "corporation"],
        "labor": ["employee", "employer", "labour", "workplace", "dismissal", "CCMA"],
        # Add more legal domains relevant to South African law
    }

    text_lower = text.lower()
    domain_scores = {}

    for domain, keywords in domains.items():
        score = sum(1 for keyword in keywords if keyword in text_lower)
        domain_scores[domain] = score

    if all(score == 0 for score in domain_scores.values()):
        return "general"

    return max(domain_scores, key=domain_scores.get)
```

```javascript
// Audio Recording Component (React Native)
// mobile/src/components/AudioRecorder.tsx
import React, { useState, useEffect } from 'react';
import { View, Text, TouchableOpacity, StyleSheet, Platform } from 'react-native';
import { Audio } from 'expo-av';
import { FontAwesome } from '@expo/vector-icons';
import { supabase } from '../lib/supabase';
import * as FileSystem from 'expo-file-system';

interface AudioRecorderProps {
  matterId: string;
  matterTitle: string;
  onRecordingComplete: (recordingData: {
    title: string,
    audioUrl: string,
    duration: number,
    matterId: string,
  }) => void;
}

export const AudioRecorder: React.FC<AudioRecorderProps> = ({ matterId, matterTitle, onRecordingComplete }) => {
  const [recording, setRecording] = (useState < Audio.Recording) | (null > null);
  const [isRecording, setIsRecording] = useState(false);
  const [duration, setDuration] = useState(0);
  const [timer, setTimer] = (useState < NodeJS.Timeout) | (null > null);
  const [recordingTitle, setRecordingTitle] = useState(`${matterTitle} - ${new Date().toLocaleDateString()}`);

  // Request permissions
  useEffect(() => {
    (async () => {
      const { status } = await Audio.requestPermissionsAsync();
      if (status !== 'granted') {
        console.error('Audio recording permission not granted');
      }
    })();

    return () => {
      if (timer) clearInterval(timer);
      if (recording) stopRecording();
    };
  }, []);

  const startRecording = async () => {
    try {
      // Configure audio
      await Audio.setAudioModeAsync({
        allowsRecordingIOS: true,
        playsInSilentModeIOS: true,
        staysActiveInBackground: true, // Enable background recording
      });

      // Start recording
      const { recording } = await Audio.Recording.createAsync(Audio.RecordingOptionsPresets.HIGH_QUALITY);

      setRecording(recording);
      setIsRecording(true);

      // Start timer
      const interval = setInterval(() => {
        setDuration(prev => prev + 1);
      }, 1000);
      setTimer(interval);
    } catch (err) {
      console.error('Failed to start recording', err);
    }
  };

  const stopRecording = async () => {
    if (!recording) return;

    try {
      // Stop recording
      await recording.stopAndUnloadAsync();

      // Clear timer
      if (timer) {
        clearInterval(timer);
        setTimer(null);
      }

      // Get recording URI
      const uri = recording.getURI();
      if (!uri) throw new Error('Recording URI is null');

      // Upload to Supabase
      const fileName = `${Date.now()}.m4a`;
      const filePath = `recordings/${matterId}/${fileName}`;

      // Read file as base64
      const fileBase64 = await FileSystem.readAsStringAsync(uri, {
        encoding: FileSystem.EncodingType.Base64,
      });

      // Upload to Supabase storage
      const { data, error } = await supabase.storage.from('legal-recordings').upload(filePath, decode(fileBase64), {
        contentType: 'audio/m4a',
        upsert: false,
      });

      if (error) throw error;

      // Get public URL or signed URL based on privacy requirements
      const { data: urlData } = await supabase.storage
        .from('legal-recordings')
        .createSignedUrl(filePath, 60 * 60 * 24 * 7); // 7-day signed URL

      // Notify parent component
      onRecordingComplete({
        title: recordingTitle,
        audioUrl: urlData.signedUrl,
        duration: duration,
        matterId: matterId,
      });

      // Reset state
      setRecording(null);
      setIsRecording(false);
      setDuration(0);
    } catch (err) {
      console.error('Failed to stop recording', err);
    }
  };

  // Helper function to decode base64
  const decode = (base64: string) => {
    return Platform.OS === 'web' ? Buffer.from(base64, 'base64') : base64;
  };

  // Format seconds to MM:SS
  const formatTime = (seconds: number) => {
    const mins = Math.floor(seconds / 60);
    const secs = seconds % 60;
    return `${mins.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Record Legal Proceeding</Text>
      <Text style={styles.matterTitle}>{matterTitle}</Text>

      {isRecording && (
        <View style={styles.durationContainer}>
          <Text style={styles.durationText}>{formatTime(duration)}</Text>
          <View style={styles.recordingIndicator} />
        </View>
      )}

      <TouchableOpacity
        style={[styles.recordButton, isRecording && styles.stopButton]}
        onPress={isRecording ? stopRecording : startRecording}>
        <FontAwesome name={isRecording ? 'stop' : 'microphone'} size={24} color='white' />
      </TouchableOpacity>

      <Text style={styles.instructionText}>
        {isRecording ? 'Recording in progress. Tap to stop.' : 'Tap to start recording legal proceeding'}
      </Text>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    padding: 20,
    alignItems: 'center',
  },
  title: {
    fontSize: 18,
    fontWeight: 'bold',
    marginBottom: 10,
  },
  matterTitle: {
    fontSize: 16,
    marginBottom: 20,
    color: '#4F46E5', // Primary color
  },
  durationContainer: {
    flexDirection: 'row',
    alignItems: 'center',
    marginBottom: 20,
  },
  durationText: {
    fontSize: 36,
    fontWeight: 'bold',
    marginRight: 10,
  },
  recordingIndicator: {
    width: 12,
    height: 12,
    borderRadius: 6,
    backgroundColor: 'red',
  },
  recordButton: {
    width: 70,
    height: 70,
    borderRadius: 35,
    backgroundColor: '#4F46E5', // Primary color
    justifyContent: 'center',
    alignItems: 'center',
    marginBottom: 20,
  },
  stopButton: {
    backgroundColor: '#EF4444', // Red color
  },
  instructionText: {
    fontSize: 14,
    color: '#6B7280', // Gray color
    textAlign: 'center',
  },
});
```

### Week 3: Legal Natural Language Interface & Export (May 21-27, 2025)

#### Detailed Task Checklist

##### Legal Chat Interface

- [ ] Design legal-focused chat UI with case law citation display
- [ ] Implement message bubbles with legal source attribution
- [ ] Add typing indicators and loading states for legal queries
- [ ] Create specialized legal input area with suggestion chips
- [ ] Implement keyboard shortcuts for legal practitioners
- [ ] Add legal source preview capability within chat

##### Legal RAG Implementation

- [ ] Set up LangChain integration with South African legal prompt templates
- [ ] Implement vector similarity search optimized for legal terminology
- [ ] Create legal context window with case law priority
- [ ] Add South African legal framework system prompts (POPIA, Companies Act, etc.)
- [ ] Implement legal response validation with source verification
- [ ] Set up response caching with legal citation indexing

##### Legal Export Functionality

- [ ] Create rich-text export format for legal documents
- [ ] Implement Word document export with proper legal formatting
- [ ] Add proper legal citation formatting in exports
- [ ] Create PDF export with legal document structure
- [ ] Implement document metadata preservation
- [ ] Add configurable export templates for different legal documents

##### Legal Source Citations

- [ ] Implement South African case law reference tracking
- [ ] Create clickable case law citations with preview
- [ ] Add statute and regulation references with verification
- [ ] Implement legal authority scoring system
- [ ] Create citation management with legal hierarchy
- [ ] Add conflict detection between cited legal sources

##### Heads of Argument Generation

- [ ] Create document structure templates for legal arguments
- [ ] Implement legal reasoning extraction from transcripts
- [ ] Add automatic legal authority suggestion
- [ ] Create legal outline generation with hierarchy
- [ ] Implement human-in-the-loop editing interface
- [ ] Add formatting for court submission

##### Mobile App Enhancement

- [ ] Integrate transcription status monitoring
- [ ] Add generated document preview in mobile
- [ ] Create recording organization by matter
- [ ] Implement secure sharing of recordings
- [ ] Add basic editing of recording metadata
- [ ] Create offline recording capability with sync

| Task                       | Owner | Status      | Due    |
| -------------------------- | ----- | ----------- | ------ |
| Legal Chat Interface       |       | Not Started | May 21 |
| Legal RAG Implementation   |       | Not Started | May 22 |
| Legal Export Functionality |       | Not Started | May 23 |
| Legal Source Citations     |       | Not Started | May 24 |
| Heads of Argument Gen      |       | Not Started | May 25 |
| Mobile App Enhancement     |       | Not Started | May 26 |

**Week 3 Milestone**: Functional legal chat interface with case law integration and document generation capabilities

#### Week 3 Technical Implementation Details

```python
# Legal RAG Implementation (simplified)
# app/services/legal_rag_processor.py
import os
from typing import List, Dict, Any
import pinecone
from langchain.embeddings.openai import OpenAIEmbeddings
from langchain.vectorstores import Pinecone
from langchain.chat_models import ChatOpenAI
from langchain.chains import RetrievalQA
from langchain.prompts import PromptTemplate
from langchain.memory import ConversationBufferMemory

def init_pinecone():
    """Initialize Pinecone connection"""
    pinecone.init(
        api_key=os.environ.get("PINECONE_API_KEY"),
        environment=os.environ.get("PINECONE_ENVIRONMENT")
    )
    return pinecone.Index(os.environ.get("PINECONE_INDEX"))

def get_sa_legal_prompt():
    """Get South African legal prompt template"""
    return """
    You are Verdict360, a specialized South African legal assistant. You provide accurate
    information based on South African law, case precedents, and legal principles.

    Use the following pieces of context to answer the user's question about South African law.
    Always cite specific cases, statutes, or regulations that support your answers.
    Properly format South African legal citations according to standard legal practice.
    If you don't know the answer, just say that you don't know, don't try to make up an answer.

    IMPORTANT SOUTH AFRICAN LEGAL CONTEXT:
    - The Constitution of the Republic of South Africa is the supreme law
    - South Africa has a mixed legal system (Roman-Dutch civil law and English common law)
    - The court hierarchy: Constitutional Court > Supreme Court of Appeal > High Courts > Magistrates' Courts
    - Key legislation includes: Companies Act 71 of 2008, POPIA of 2013, Labour Relations Act 66 of 1995

    Provided Context:
    {context}

    Chat History:
    {chat_history}

    User Question: {question}

    Answer (with proper South African legal citations):
    """

def generate_legal_response(query: str, conversation_history: List[Dict[str, str]], jurisdiction: str = "South Africa"):
    """Generate RAG response with South African legal context"""
    # Initialize vector store
    pinecone_index = init_pinecone()
    embeddings = OpenAIEmbeddings()
    vectorstore = Pinecone(
        index=pinecone_index,
        embedding=embeddings,
        text_key="text"
    )

    # Create conversation memory
    memory = ConversationBufferMemory(
        memory_key="chat_history",
        return_messages=True
    )

    # Add conversation history to memory
    for message in conversation_history:
        if message["role"] == "user":
            memory.chat_memory.add_user_message(message["content"])
        else:
            memory.chat_memory.add_ai_message(message["content"])

    # Create South African legal context-aware prompt
    PROMPT = PromptTemplate(
        template=get_sa_legal_prompt(),
        input_variables=["context", "chat_history", "question"]
    )

    # Create retrieval chain with legal optimization
    llm = ChatOpenAI(
        model_name="gpt-4-turbo",
        temperature=0.2,
    )

    # Specialized retrieval with legal metadata filtering
    retriever = vectorstore.as_retriever(
        search_kwargs={
            "k": 8,  # Retrieve more documents for legal context
            "filter": {"jurisdiction": jurisdiction}  # Filter by jurisdiction
        }
    )

    qa_chain = RetrievalQA.from_chain_type(
        llm=llm,
        chain_type="stuff",
        retriever=retriever,
        return_source_documents=True,
        chain_type_kwargs={
            "verbose": True,
            "prompt": PROMPT,
            "memory": memory,
        }
    )

    # Generate response
    result = qa_chain({"query": query})

    # Process citations and sources
    processed_result = process_legal_citations(result)

    return processed_result

def process_legal_citations(result: Dict[str, Any]) -> Dict[str, Any]:
    """Process and structure legal citations from response"""
    # Extract source documents
    source_docs = result.get("source_documents", [])

    # Extract and validate citations
    citations = []
    for doc in source_docs:
        # Extract metadata
        metadata = doc.metadata

        # Add citation information
        if "citations" in metadata and metadata["citations"]:
            for citation in metadata["citations"]:
                citations.append({
                    "text": citation,
                    "document_id": metadata.get("document_id", ""),
                    "page": metadata.get("page", ""),
                    "legal_domain": metadata.get("legal_domain", "general"),
                    "verified": True  # In production, would verify against legal database
                })

    # Remove duplicates while preserving order
    unique_citations = []
    seen = set()
    for citation in citations:
        citation_text = citation["text"]
        if citation_text not in seen:
            seen.add(citation_text)
            unique_citations.append(citation)

    # Structure the final response
    return {
        "response": result["result"],
        "sources": unique_citations,
        "raw_source_documents": source_docs
    }
```

```typescript
// Rich-Text Export Component
// components/legal/ExportLegalDocument.tsx
import React, { useState } from 'react';
import {
  Button,
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
  Card,
  CardContent,
  CardFooter,
  CardHeader,
  CardTitle,
} from '@/components/ui';
import { Download, FileText, FileWord, FilePdf } from 'lucide-react';
import { exportToWord, exportToPdf, exportToText } from '@/lib/utils/export';
import { LegalCitation } from '@/types/legal';

interface ExportLegalDocumentProps {
  content: string;
  title: string;
  citations: LegalCitation[];
  matterReference?: string;
  documentType: 'transcript' | 'summary' | 'heads' | 'opinion';
}

const legalTemplates = {
  standard: 'Standard Legal Document',
  heads: 'Heads of Argument',
  opinion: 'Legal Opinion',
  transcript: 'Meeting Transcript',
  summary: 'Legal Summary',
};

export const ExportLegalDocument: React.FC<ExportLegalDocumentProps> = ({
  content,
  title,
  citations,
  matterReference,
  documentType,
}) => {
  const [template, setTemplate] = useState(documentType);
  const [loading, setLoading] = useState(false);
  const [format, setFormat] = useState<'docx' | 'pdf' | 'txt'>('docx');

  const handleExport = async () => {
    try {
      setLoading(true);

      // Format citations according to South African legal citation style
      const formattedCitations = formatLegalCitations(citations);

      // Format content based on template
      const formattedContent = formatLegalContent(content, title, formattedCitations, matterReference, template);

      // Export based on selected format
      switch (format) {
        case 'docx':
          await exportToWord(formattedContent, title);
          break;
        case 'pdf':
          await exportToPdf(formattedContent, title);
          break;
        case 'txt':
          await exportToText(formattedContent, title);
          break;
      }
    } catch (error) {
      console.error('Export failed:', error);
      // Show error notification
    } finally {
      setLoading(false);
    }
  };

  return (
    <Card>
      <CardHeader>
        <CardTitle className='text-primary'>Export Legal Document</CardTitle>
      </CardHeader>
      <CardContent>
        <div className='space-y-4'>
          <div>
            <label className='text-sm font-medium'>Document Template</label>
            <Select value={template} onValueChange={(value: any) => setTemplate(value)}>
              <SelectTrigger>
                <SelectValue placeholder='Select template' />
              </SelectTrigger>
              <SelectContent>
                {Object.entries(legalTemplates).map(([value, label]) => (
                  <SelectItem key={value} value={value}>
                    {label}
                  </SelectItem>
                ))}
              </SelectContent>
            </Select>
          </div>

          <div>
            <label className='text-sm font-medium'>Export Format</label>
            <div className='flex space-x-2 mt-2'>
              <Button variant={format === 'docx' ? 'default' : 'outline'} size='sm' onClick={() => setFormat('docx')}>
                <FileWord className='mr-2 h-4 w-4' />
                Word
              </Button>
              <Button variant={format === 'pdf' ? 'default' : 'outline'} size='sm' onClick={() => setFormat('pdf')}>
                <FilePdf className='mr-2 h-4 w-4' />
                PDF
              </Button>
              <Button variant={format === 'txt' ? 'default' : 'outline'} size='sm' onClick={() => setFormat('txt')}>
                <FileText className='mr-2 h-4 w-4' />
                Text
              </Button>
            </div>
          </div>

          <div className='border rounded p-3 bg-muted/30'>
            <h3 className='text-sm font-medium mb-2'>Document Information</h3>
            <p className='text-sm'>
              <strong>Title:</strong> {title}
            </p>
            <p className='text-sm'>
              <strong>Citations:</strong> {citations.length}
            </p>
            {matterReference && (
              <p className='text-sm'>
                <strong>Matter Ref:</strong> {matterReference}
              </p>
            )}
          </div>
        </div>
      </CardContent>
      <CardFooter>
        <Button onClick={handleExport} disabled={loading}>
          <Download className='mr-2 h-4 w-4' />
          {loading ? 'Exporting...' : 'Export Document'}
        </Button>
      </CardFooter>
    </Card>
  );
};

// Helper functions
function formatLegalCitations(citations: LegalCitation[]): string {
  if (!citations.length) return '';

  return citations.map(citation => `${citation.text} ${citation.page ? `at ${citation.page}` : ''}`).join('\n');
}

function formatLegalContent(
  content: string,
  title: string,
  citations: string,
  matterReference?: string,
  template: string = 'standard'
): string {
  // Basic template for legal documents with proper formatting
  // In production would use more sophisticated templating

  const headerSection = `
    ${title.toUpperCase()}
    ${matterReference ? `REFERENCE: ${matterReference}` : ''}
    DATE: ${new Date().toISOString().split('T')[0]}
  `;

  const citationSection = citations
    ? `
    AUTHORITIES CITED:
    ${citations}
  `
    : '';

  // Different templates based on document type
  switch (template) {
    case 'heads':
      return `
        ${headerSection}
        
        HEADS OF ARGUMENT
        
        ${content}
        
        ${citationSection}
      `;

    case 'opinion':
      return `
        ${headerSection}
        
        LEGAL OPINION
        
        ${content}
        
        ${citationSection}
      `;

    case 'transcript':
      return `
        ${headerSection}
        
        TRANSCRIPT OF PROCEEDINGS
        
        ${content}
      `;

    case 'summary':
      return `
        ${headerSection}
        
        LEGAL SUMMARY
        
        ${content}
        
        ${citationSection}
      `;

    default:
      return `
        ${headerSection}
        
        ${content}
        
        ${citationSection}
      `;
  }
}
```

### Week 4: Polish, Testing & Launch (May 28-June 3, 2025)

#### Detailed Task Checklist

##### Legal Admin Dashboard

- [ ] Create usage statistics dashboard for legal firms
- [ ] Implement document analytics by practice area
- [ ] Add user activity monitoring with legal matter context
- [ ] Create system health indicators for legal operations
- [ ] Implement API usage tracking with cost estimation
- [ ] Add legal resource allocation visualization

##### Legal Quality Assurance

- [ ] Implement legal response verification against sources
- [ ] Create test suite for South African legal queries
- [ ] Add citation accuracy verification
- [ ] Implement legal domain classification validation
- [ ] Create confidence scoring for legal responses
- [ ] Add legal compliance checks for outputs

##### Performance Optimization

- [ ] Optimize vector search for legal terminology
- [ ] Implement caching for frequent legal queries
- [ ] Add batch processing for document ingestion
- [ ] Optimize mobile app for bandwidth efficiency
- [ ] Implement progressive loading of legal documents
- [ ] Add background processing for transcription jobs

##### Legal Professional Testing

- [ ] Create testing script with South African legal scenarios
- [ ] Set up test environment with South African legal datasets
- [ ] Recruit 3-5 South African legal professionals for testing
- [ ] Conduct structured testing sessions with attorneys
- [ ] Document feedback from legal practitioners
- [ ] Implement critical fixes based on legal expertise

##### Responsive Design & User Experience

- [ ] Test UI across desktop, tablet, and mobile devices
- [ ] Optimize legal document viewing on mobile
- [ ] Ensure audio playback works across devices
- [ ] Fix touch interaction issues for court settings
- [ ] Optimize loading performance for legal documents
- [ ] Create legal-specific mobile layouts

##### Deployment & Launch Preparation

- [ ] Set up production environment with legal compliance
- [ ] Configure DNS and SSL with security certificates
- [ ] Set up monitoring for legal API usage
- [ ] Create deployment documentation for legal IT teams
- [ ] Perform security audit with legal data considerations
- [ ] Execute production deployment with staged rollout

| Task                       | Owner | Status      | Due    |
| -------------------------- | ----- | ----------- | ------ |
| Legal Admin Dashboard      |       | Not Started | May 28 |
| Legal Quality Assurance    |       | Not Started | May 29 |
| Performance Optimization   |       | Not Started | May 30 |
| Legal Professional Testing |       | Not Started | May 31 |
| Responsive Design & UX     |       | Not Started | June 2 |
| Deployment & Launch        |       | Not Started | June 3 |

**Week 4 Milestone**: Production-ready MVP deployed for initial legal firm testing with full legal citation and document generation functionality

#### Week 4 Technical Implementation Details

```typescript
// Legal Admin Dashboard Component
// components/admin/LegalDashboard.tsx
import React, { useState, useEffect } from 'react';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { Tabs, TabsContent, TabsList, TabsTrigger } from '@/components/ui/tabs';
import {
  BarChart,
  Bar,
  XAxis,
  YAxis,
  CartesianGrid,
  Tooltip,
  Legend,
  ResponsiveContainer,
  PieChart,
  Pie,
  Cell,
} from 'recharts';
import { supabase } from '@/lib/supabase';
import { formatDistanceToNow } from 'date-fns';

// Colors for charts
const COLORS = ['#4F46E5', '#059669', '#8B5CF6', '#F59E0B', '#EF4444'];
const LEGAL_DOMAINS = ['Constitutional', 'Criminal', 'Civil', 'Corporate', 'Labor'];

export default function LegalDashboard() {
  const [activeTab, setActiveTab] = useState('overview');
  const [usageStats, setUsageStats] = useState<any>(null);
  const [documentsStats, setDocumentsStats] = useState<any>(null);
  const [recordingsStats, setRecordingsStats] = useState<any>(null);
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    const fetchStats = async () => {
      try {
        // Fetch overall usage statistics
        const { data: usageData, error: usageError } = await supabase.from('admin_statistics').select('*').single();

        if (usageError) throw usageError;

        // Fetch document statistics by practice area
        const { data: documentsData, error: documentsError } = await supabase
          .from('legal_documents')
          .select('document_type, created_at')
          .order('created_at', { ascending: false });

        if (documentsError) throw documentsError;

        // Fetch recording statistics
        const { data: recordingsData, error: recordingsError } = await supabase
          .from('legal_recordings')
          .select('id, title, created_at, duration_seconds, transcription_status');

        if (recordingsError) throw recordingsError;

        // Process the data
        setUsageStats(usageData);

        // Group documents by type
        const documentsByType = documentsData.reduce((acc: any, doc: any) => {
          const type = doc.document_type || 'Other';
          if (!acc[type]) acc[type] = 0;
          acc[type]++;
          return acc;
        }, {});

        setDocumentsStats({
          byType: Object.entries(documentsByType).map(([name, value]) => ({ name, value })),
          recent: documentsData.slice(0, 5),
        });

        // Process recordings data
        const transcriptionStatusCount = recordingsData.reduce((acc: any, rec: any) => {
          const status = rec.transcription_status || 'unknown';
          if (!acc[status]) acc[status] = 0;
          acc[status]++;
          return acc;
        }, {});

        setRecordingsStats({
          statusCount: Object.entries(transcriptionStatusCount).map(([name, value]) => ({ name, value })),
          recent: recordingsData.slice(0, 5),
          totalDuration: recordingsData.reduce((sum: number, rec: any) => sum + (rec.duration_seconds || 0), 0),
        });

        setIsLoading(false);
      } catch (error) {
        console.error('Error fetching stats:', error);
        setIsLoading(false);
      }
    };

    fetchStats();
  }, []);

  if (isLoading) {
    return <div className='p-8 text-center'>Loading legal dashboard data...</div>;
  }

  return (
    <div className='p-4 md:p-8 space-y-6'>
      <h1 className='text-2xl font-bold text-primary'>Legal Practice Dashboard</h1>

      <Tabs value={activeTab} onValueChange={setActiveTab}>
        <TabsList className='grid grid-cols-3 mb-6'>
          <TabsTrigger value='overview'>Overview</TabsTrigger>
          <TabsTrigger value='documents'>Legal Documents</TabsTrigger>
          <TabsTrigger value='recordings'>Recordings</TabsTrigger>
        </TabsList>

        <TabsContent value='overview'>
          <div className='grid grid-cols-1 md:grid-cols-3 gap-6'>
            <Card>
              <CardHeader className='pb-2'>
                <CardTitle className='text-sm font-medium'>Total Legal Documents</CardTitle>
              </CardHeader>
              <CardContent>
                <div className='text-3xl font-bold'>{usageStats?.total_documents || 0}</div>
                <p className='text-xs text-muted-foreground mt-1'>+{usageStats?.documents_today || 0} today</p>
              </CardContent>
            </Card>

            <Card>
              <CardHeader className='pb-2'>
                <CardTitle className='text-sm font-medium'>Total Legal Queries</CardTitle>
              </CardHeader>
              <CardContent>
                <div className='text-3xl font-bold'>{usageStats?.total_queries || 0}</div>
                <p className='text-xs text-muted-foreground mt-1'>+{usageStats?.queries_today || 0} today</p>
              </CardContent>
            </Card>

            <Card>
              <CardHeader className='pb-2'>
                <CardTitle className='text-sm font-medium'>Recorded Minutes</CardTitle>
              </CardHeader>
              <CardContent>
                <div className='text-3xl font-bold'>{Math.floor((recordingsStats?.totalDuration || 0) / 60)}</div>
                <p className='text-xs text-muted-foreground mt-1'>
                  Across {recordingsStats?.recent.length || 0} recordings
                </p>
              </CardContent>
            </Card>
          </div>

          <div className='grid grid-cols-1 md:grid-cols-2 gap-6 mt-6'>
            <Card>
              <CardHeader>
                <CardTitle className='text-sm font-medium'>Legal Domains Usage</CardTitle>
              </CardHeader>
              <CardContent>
                <ResponsiveContainer width='100%' height={300}>
                  <BarChart
                    data={LEGAL_DOMAINS.map(domain => ({
                      name: domain,
                      queries: Math.floor(Math.random() * 100), // In production, use real data
                    }))}
                    margin={{ top: 20, right: 30, left: 20, bottom: 5 }}>
                    <CartesianGrid strokeDasharray='3 3' />
                    <XAxis dataKey='name' />
                    <YAxis />
                    <Tooltip />
                    <Bar dataKey='queries' fill='#4F46E5' />
                  </BarChart>
                </ResponsiveContainer>
              </CardContent>
            </Card>

            <Card>
              <CardHeader>
                <CardTitle className='text-sm font-medium'>Recent Activity</CardTitle>
              </CardHeader>
              <CardContent>
                <div className='space-y-4'>
                  {(documentsStats?.recent || []).map((doc: any, i: number) => (
                    <div key={i} className='flex items-center'>
                      <div className='w-2 h-2 rounded-full bg-primary mr-2'></div>
                      <div className='flex-1 text-sm'>New {doc.document_type} document added</div>
                      <div className='text-xs text-muted-foreground'>
                        {formatDistanceToNow(new Date(doc.created_at), { addSuffix: true })}
                      </div>
                    </div>
                  ))}
                </div>
              </CardContent>
            </Card>
          </div>
        </TabsContent>

        <TabsContent value='documents'>
          <div className='grid grid-cols-1 md:grid-cols-2 gap-6'>
            <Card>
              <CardHeader>
                <CardTitle className='text-sm font-medium'>Documents by Type</CardTitle>
              </CardHeader>
              <CardContent>
                <ResponsiveContainer width='100%' height={300}>
                  <PieChart>
                    <Pie
                      data={documentsStats?.byType || []}
                      cx='50%'
                      cy='50%'
                      labelLine={false}
                      outerRadius={80}
                      fill='#8884d8'
                      dataKey='value'
                      label={({ name, percent }) => `${name} ${(percent * 100).toFixed(0)}%`}>
                      {(documentsStats?.byType || []).map((entry: any, index: number) => (
                        <Cell key={`cell-${index}`} fill={COLORS[index % COLORS.length]} />
                      ))}
                    </Pie>
                    <Tooltip />
                  </PieChart>
                </ResponsiveContainer>
              </CardContent>
            </Card>

            <Card>
              <CardHeader>
                <CardTitle className='text-sm font-medium'>Recent Documents</CardTitle>
              </CardHeader>
              <CardContent>
                <div className='space-y-4'>
                  {(documentsStats?.recent || []).map((doc: any, i: number) => (
                    <div key={i} className='flex items-center justify-between border-b pb-2'>
                      <div className='text-sm font-medium'>{doc.document_type || 'Document'}</div>
                      <div className='text-xs text-muted-foreground'>
                        {formatDistanceToNow(new Date(doc.created_at), { addSuffix: true })}
                      </div>
                    </div>
                  ))}
                </div>
              </CardContent>
            </Card>
          </div>
        </TabsContent>

        <TabsContent value='recordings'>
          <div className='grid grid-cols-1 md:grid-cols-2 gap-6'>
            <Card>
              <CardHeader>
                <CardTitle className='text-sm font-medium'>Transcription Status</CardTitle>
              </CardHeader>
              <CardContent>
                <ResponsiveContainer width='100%' height={300}>
                  <PieChart>
                    <Pie
                      data={recordingsStats?.statusCount || []}
                      cx='50%'
                      cy='50%'
                      labelLine={false}
                      outerRadius={80}
                      fill='#8884d8'
                      dataKey='value'
                      label={({ name, percent }) => `${name} ${(percent * 100).toFixed(0)}%`}>
                      {(recordingsStats?.statusCount || []).map((entry: any, index: number) => (
                        <Cell key={`cell-${index}`} fill={COLORS[index % COLORS.length]} />
                      ))}
                    </Pie>
                    <Tooltip />
                  </PieChart>
                </ResponsiveContainer>
              </CardContent>
            </Card>

            <Card>
              <CardHeader>
                <CardTitle className='text-sm font-medium'>Recent Recordings</CardTitle>
              </CardHeader>
              <CardContent>
                <div className='space-y-4'>
                  {(recordingsStats?.recent || []).map((rec: any, i: number) => (
                    <div key={i} className='flex flex-col border-b pb-2'>
                      <div className='flex items-center justify-between'>
                        <div className='text-sm font-medium'>{rec.title}</div>
                        <div className='text-xs bg-primary/10 text-primary rounded px-2 py-0.5'>
                          {rec.transcription_status}
                        </div>
                      </div>
                      <div className='text-xs text-muted-foreground mt-1'>
                        Duration: {Math.floor(rec.duration_seconds / 60)}:
                        {(rec.duration_seconds % 60).toString().padStart(2, '0')} •
                        {formatDistanceToNow(new Date(rec.created_at), { addSuffix: true })}
                      </div>
                    </div>
                  ))}
                </div>
              </CardContent>
            </Card>
          </div>
        </TabsContent>
      </Tabs>
    </div>
  );
}
```

```python
# Legal Quality Assurance Utility
# app/utils/legal_qa.py
import re
from typing import Dict, List, Any, Tuple

# South African case citation patterns for validation
SA_CITATION_PATTERNS = {
    'case_law': [
        r'\d{4}\s+\(\d+\)\s+SA\s+\d+\s+\([A-Z]+\)',  # 2019 (2) SA 343 (SCA)
        r'\[\d{4}\]\s+ZACC\s+\d+',                   # [2021] ZACC 13
        r'\[\d{4}\]\s+ZASCA\s+\d+',                  # [2020] ZASCA 99
        r'\d{4}\s+\(\d+\)\s+BCLR\s+\d+',             # 2018 (7) BCLR 844
    ],
    'statutes': [
        r'Act\s+No\.\s+\d+\s+of\s+\d{4}',            # Act No. 71 of 2008
        r'Act\s+\d+\s+of\s+\d{4}',                   # Act 71 of 2008
    ],
    'regulations': [
        r'GN\s+R\s+\d+',                             # GN R 1234
        r'GG\s+\d+',                                 # GG 43834
    ]
}

# Primary South African legal authorities for validation
PRIMARY_AUTHORITIES = {
    'constitution': [
        'Constitution of the Republic of South Africa, 1996',
        'Constitutional',
        'Constitution',
        'Bill of Rights'
    ],
    'key_statutes': [
        'Companies Act 71 of 2008',
        'Protection of Personal Information Act 4 of 2013',
        'POPIA',
        'Labour Relations Act 66 of 1995',
        'Consumer Protection Act 68 of 2008',
        'National Credit Act 34 of 2005'
    ]
}

def validate_legal_citations(text: str) -> Dict[str, Any]:
    """Validate South African legal citations in a text response"""
    citation_count = 0
    citation_types = {
        'case_law': 0,
        'statutes': 0,
        'regulations': 0,
        'constitution': 0
    }

    found_citations = []

    # Check for case law citations
    for pattern in SA_CITATION_PATTERNS['case_law']:
        matches = re.findall(pattern, text)
        for match in matches:
            citation_count += 1
            citation_types['case_law'] += 1
            found_citations.append({
                'text': match,
                'type': 'case_law',
                'verified': True  # Would verify against actual database
            })

    # Check for statute citations
    for pattern in SA_CITATION_PATTERNS['statutes']:
        matches = re.findall(pattern, text)
        for match in matches:
            citation_count += 1
            citation_types['statutes'] += 1
            found_citations.append({
                'text': match,
                'type': 'statute',
                'verified': True
            })

    # Check for regulation citations
    for pattern in SA_CITATION_PATTERNS['regulations']:
        matches = re.findall(pattern, text)
        for match in matches:
            citation_count += 1
            citation_types['regulations'] += 1
            found_citations.append({
                'text': match,
                'type': 'regulation',
                'verified': True
            })

    # Check for constitutional references
    for term in PRIMARY_AUTHORITIES['constitution']:
        if term in text:
            citation_count += 1
            citation_types['constitution'] += 1
            found_citations.append({
                'text': term,
                'type': 'constitution',
                'verified': True
            })
            break  # Only count constitution once

    # Calculate citation diversity score (0-1)
    citation_diversity = sum(1 for count in citation_types.values() if count > 0) / len(citation_types)

    return {
        'citation_count': citation_count,
        'citation_types': citation_types,
        'citations': found_citations,
        'citation_diversity': citation_diversity,
        'has_citations': citation_count > 0
    }

def assess_legal_response_quality(response: str, query: str) -> Dict[str, Any]:
    """Assess the quality of a legal response"""
    # Validate citations
    citation_data = validate_legal_citations(response)

    # Check for South African legal terminology
    sa_legal_terms = [
        'Constitutional Court', 'Supreme Court of Appeal', 'High Court',
        'Magistrates\' Court', 'Roman-Dutch', 'common law', 'delict',
        'ubuntu', 'stare decisis', 'constitutional democracy'
    ]

    legal_term_count = sum(1 for term in sa_legal_terms if term.lower() in response.lower())

    # Check for hedging language (unwanted in legal opinions)
    hedging_terms = [
        'might be', 'could be', 'perhaps', 'possibly', 'maybe',
        'I think', 'probably', 'seems like', 'appears to be'
    ]

    hedging_count = sum(1 for term in hedging_terms if term.lower() in response.lower())

    # Check for definitive language (wanted in legal opinions)
    definitive_terms = [
        'must', 'shall', 'required', 'necessary', 'obligatory',
        'mandatory', 'prohibited', 'unlawful', 'illegal', 'entitled'
    ]

    definitive_count = sum(1 for term in definitive_terms if term.lower() in response.lower())

    # Calculate overall quality score
    quality_components = {
        'citation_score': min(citation_data['citation_count'] / 3, 1.0),  # Normalize to max 1.0
        'citation_diversity': citation_data['citation_diversity'],
        'legal_terminology': min(legal_term_count / 5, 1.0),  # Normalize to max 1.0
        'hedging_penalty': max(0, 1 - (hedging_count / 5)),  # Less hedging is better
        'definitive_language': min(definitive_count / 5, 1.0)  # More definitive language is better
    }

    overall_quality = sum(quality_components.values()) / len(quality_components)

    return {
        'citation_analysis': citation_data,
        'legal_terminology_count': legal_term_count,
        'hedging_language_count': hedging_count,
        'definitive_language_count': definitive_count,
        'quality_components': quality_components,
        'overall_quality': overall_quality,
        'quality_assessment': get_quality_assessment(overall_quality)
    }

def get_quality_assessment(quality_score: float) -> str:
    """Get a qualitative assessment based on quality score"""
    if quality_score >= 0.8:
        return "Excellent legal response with proper citations and terminology"
    elif quality_score >= 0.6:
        return "Good legal response with some citations"
    elif quality_score >= 0.4:
        return "Acceptable legal response but needs more citations"
    else:
        return "Insufficient legal quality - requires improvement with proper citations"
```
