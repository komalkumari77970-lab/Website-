# Website-import React, { useState, useRef } from 'react';
import { Button } from "/components/ui/button";
import { Card, CardContent, CardDescription, CardFooter, CardHeader, CardTitle } from "/components/ui/card";
import { Input } from "/components/ui/input";
import { Label } from "/components/ui/label";
const PDFImageCompressor = () => {
  const [selectedFile, setSelectedFile] = useState<File | null>(null);
  const [compressionLevel, setCompressionLevel] = useState<number>(70);
  const [isCompressing, setIsCompressing] = useState<boolean>(false);
  const [isCompressed, setIsCompressed] = useState<boolean>(false);
  const fileInputRef = useRef<HTMLInputElement>(null);
  const handleFileChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    const file = event.target.files?.[0];
    if (file) {
      setSelectedFile(file);
      setIsCompressed(false);
    }
  };
  const handleDrop = (event: React.DragEvent<HTMLDivElement>) => {
    event.preventDefault();
    const file = event.dataTransfer.files[0];
    if (file && (file.type.includes('image/') || file.type === 'application/pdf')) {
      setSelectedFile(file);
      setIsCompressed(false);
    }
  };
  const handleDragOver = (event: React.DragEvent<HTMLDivElement>) => {
    event.preventDefault();
  };
  const handleCompress = () => {
    if (!selectedFile) return;
    
    setIsCompressing(true);
    // Simulate compression process
    setTimeout(() => {
      setIsCompressing(false);
      setIsCompressed(true);
    }, 2000);
  };
  const handleDownload = () => {
    // Simulate download functionality
    alert('Download functionality would be implemented here');
  };
  const formatFileSize = (bytes: number): string => {
    if (bytes === 0) return '0 Bytes';
    const k = 1024;
    const sizes = ['Bytes', 'KB', 'MB', 'GB'];
    const i = Math.floor(Math.log(bytes) / Math.log(k));
    return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
  };
  const getFileType = (file: File): string => {
    if (file.type.includes('image/')) return 'Image';
    if (file.type === 'application/pdf') return 'PDF';
    return 'File';
  };
  return (
    <div className="min-h-screen bg-gradient-to-br from-blue-50 to-red-50">
      {/* Header */}
      <header className="bg-white shadow-sm border-b">
        <div className="container mx-auto px-4 py-4">
          <div className="flex items-center justify-between">
            <div className="flex items-center space-x-2">
              <div className="w-8 h-8 bg-blue-600 rounded-full"></div>
              <h1 className="text-2xl font-bold text-gray-800">Compressify</h1>
            </div>
            <nav className="hidden md:flex space-x-6">
              <a href="#" className="text-gray-600 hover:text-blue-600 transition-colors">Home</a>
              <a href="#" className="text-gray-600 hover:text-blue-600 transition-colors">Features</a>
              <a href="#" className="text-gray-600 hover:text-blue-600 transition-colors">Pricing</a>
              <a href="#" className="text-gray-600 hover:text-blue-600 transition-colors">Contact</a>
            </nav>
            <Button variant="outline" className="border-blue-600 text-blue-600 hover:bg-blue-600 hover:text-white">
              Sign In
            </Button>
          </div>
        </div>
      </header>
      {/* Hero Section */}
      <section className="py-12 bg-gradient-to-r from-blue-600 to-red-600 text-white">
        <div className="container mx-auto px-4 text-center">
          <h2 className="text-4xl font-bold mb-4">Compress PDFs & Images Instantly</h2>
          <p className="text-xl mb-8 max-w-2xl mx-auto">
            Reduce file sizes without losing quality. Fast, secure, and completely free.
          </p>
          <div className="flex flex-col sm:flex-row gap-4 justify-center">
            <Button className="bg-white text-blue-600 hover:bg-gray-100 px-8 py-3 text-lg font-semibold">
              Compress PDF
            </Button>
            <Button className="bg-white text-red-600 hover:bg-gray-100 px-8 py-3 text-lg font-semibold">
              Compress Image
            </Button>
          </div>
        </div>
      </section>
      {/* Main Content */}
      <main className="container mx-auto px-4 py-12">
        <div className="grid grid-cols-1 lg:grid-cols-2 gap-8">
          {/* Upload Section */}
          <Card className="border-2 border-dashed border-blue-300 bg-blue-50">
            <CardHeader>
              <CardTitle className="text-blue-700">Upload Your File</CardTitle>
              <CardDescription>
                Drag and drop your PDF or image file, or click to browse
              </CardDescription>
            </CardHeader>
            <CardContent>
              <div
                className="border-2 border-dashed border-blue-400 rounded-lg p-8 text-center cursor-pointer hover:bg-blue-100 transition-colors"
                onDrop={handleDrop}
                onDragOver={handleDragOver}
                onClick={() => fileInputRef.current?.click()}
              >
                <div className="w-16 h-16 bg-blue-200 rounded-full flex items-center justify-center mx-auto mb-4">
                  <svg className="w-8 h-8 text-blue-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12" />
                  </svg>
                </div>
                <p className="text-blue-700 font-medium">Drop files here or click to upload</p>
                <p className="text-blue-500 text-sm mt-2">Supports PDF, JPG, PNG, GIF (Max 100MB)</p>
                <Input
                  ref={fileInputRef}
                  type="file"
                  className="hidden"
                  accept=".pdf,.jpg,.jpeg,.png,.gif"
                  onChange={handleFileChange}
                />
              </div>
              {selectedFile && (
                <div className="mt-6 p-4 bg-white rounded-lg border border-blue-200">
                  <div className="flex items-center justify-between">
                    <div className="flex items-center space-x-3">
                      <div className="w-10 h-10 bg-blue-100 rounded flex items-center justify-center">
                        <svg className="w-6 h-6 text-blue-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                          <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" />
                        </svg>
                      </div>
                      <div>
                        <p className="font-medium text-gray-800">{selectedFile.name}</p>
                        <p className="text-sm text-gray-500">
                          {getFileType(selectedFile)} • {formatFileSize(selectedFile.size)}
                        </p>
                      </div>
                    </div>
                    <Button
                      variant="ghost"
                      className="text-red-600 hover:text-red-700 hover:bg-red-50"
                      onClick={() => setSelectedFile(null)}
                    >
                      Remove
                    </Button>
                  </div>
                </div>
              )}
            </CardContent>
          </Card>
          {/* Compression Settings */}
          <Card className="bg-white">
            <CardHeader>
              <CardTitle className="text-gray-800">Compression Settings</CardTitle>
              <CardDescription>
                Adjust compression level to balance quality and file size
              </CardDescription>
            </CardHeader>
            <CardContent className="space-y-6">
              <div>
                <Label htmlFor="compression-level" className="text-gray-700">
                  Compression Level: {compressionLevel}%
                </Label>
                <div className="flex items-center space-x-4 mt-2">
                  <span className="text-sm text-gray-500">Smaller Size</span>
                  <Input
                    id="compression-level"
                    type="range"
                    min="10"
                    max="90"
                    value={compressionLevel}
                    onChange={(e) => setCompressionLevel(Number(e.target.value))}
                    className="w-full"
                  />
                  <span className="text-sm text-gray-500">Better Quality</span>
                </div>
              </div>
              <div className="grid grid-cols-2 gap-4">
                <div className="p-4 bg-blue-50 rounded-lg border border-blue-200">
                  <h4 className="font-medium text-blue-700 mb-2">Original</h4>
                  {selectedFile ? (
                    <p className="text-blue-600">{formatFileSize(selectedFile.size)}</p>
                  ) : (
                    <p className="text-blue-400">No file selected</p>
                  )}
                </div>
                <div className="p-4 bg-red-50 rounded-lg border border-red-200">
                  <h4 className="font-medium text-red-700 mb-2">Compressed</h4>
                  {selectedFile && isCompressed ? (
                    <p className="text-red-600">
                      {formatFileSize(selectedFile.size * (1 - compressionLevel / 100))}
                      <span className="text-green-600 ml-2">
                        ({(compressionLevel / 100 * 100).toFixed(0)}% smaller)
                      </span>
                    </p>
                  ) : (
                    <p className="text-red-400">Not compressed yet</p>
                  )}
                </div>
              </div>
              <Button
                className="w-full bg-blue-600 hover:bg-blue-700 text-white py-3 text-lg font-semibold"
                disabled={!selectedFile || isCompressing}
                onClick={handleCompress}
              >
                {isCompressing ? (
                  <div className="flex items-center justify-center">
                    <div className="w-5 h-5 border-2 border-white border-t-transparent rounded-full animate-spin mr-2"></div>
                    Compressing...
                  </div>
                ) : (
                  'Compress Now'
                )}
              </Button>
              {isCompressed && (
                <Button
                  className="w-full bg-red-600 hover:bg-red-700 text-white py-3 text-lg font-semibold mt-4"
                  onClick={handleDownload}
                >
                  Download Compressed File
                </Button>
              )}
            </CardContent>
          </Card>
        </div>
        {/* Features Section */}
        <section className="mt-16">
          <h2 className="text-3xl font-bold text-center text-gray-800 mb-12">Why Choose Our Compressor?</h2>
          <div className="grid grid-cols-1 md:grid-cols-3 gap-8">
            <Card className="text-center border-blue-200">
              <CardHeader>
                <div className="w-16 h-16 bg-blue-100 rounded-full flex items-center justify-center mx-auto mb-4">
                  <svg className="w-8 h-8 text-blue-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M13 10V3L4 14h7v7l9-11h-7z" />
                  </svg>
                </div>
                <CardTitle className="text-blue-700">Lightning Fast</CardTitle>
              </CardHeader>
              <CardContent>
                <p className="text-gray-600">
                  Compress files in seconds with our optimized algorithms. No waiting, no delays.
                </p>
              </CardContent>
            </Card>
            <Card className="text-center border-red-200">
              <CardHeader>
                <div className="w-16 h-16 bg-red-100 rounded-full flex items-center justify-center mx-auto mb-4">
                  <svg className="w-8 h-8 text-red-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z" />
                  </svg>
                </div>
                <CardTitle className="text-red-700">100% Secure</CardTitle>
              </CardHeader>
              <CardContent>
                <p className="text-gray-600">
                  Your files are processed securely and never stored on our servers. Complete privacy guaranteed.
                </p>
              </CardContent>
            </Card>
            <Card className="text-center border-blue-200">
              <CardHeader>
                <div className="w-16 h-16 bg-blue-100 rounded-full flex items-center justify-center mx-auto mb-4">
                  <svg className="w-8 h-8 text-blue-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M9.663 17h4.673M12 3v1m6.364 1.636l-.707.707M21 12h-1M4 12H3m3.343-5.657l-.707-.707m2.828 9.9a5 5 0 117.072 0l-.548.547A3.374 3.374 0 0014 18.469V19a2 2 0 11-4 0v-.531c0-.895-.356-1.754-.988-2.386l-.548-.547z" />
                  </svg>
                </div>
                <CardTitle className="text-blue-700">High Quality</CardTitle>
              </CardHeader>
              <CardContent>
                <p className="text-gray-600">
                  Maintain excellent quality while significantly reducing file size. Perfect for web and email.
                </p>
              </CardContent>
            </Card>
          </div>
        </section>
      </main>
      {/* Footer */}
      <footer className="bg-gray-800 text-white py-12 mt-16">
        <div className="container mx-auto px-4">
          <div className="grid grid-cols-1 md:grid-cols-4 gap-8">
            <div>
              <h3 className="text-lg font-semibold mb-4">Compressify</h3>
              <p className="text-gray-400">
                The fastest way to compress your PDF and image files online.
              </p>
            </div>
            <div>
              <h3 className="text-lg font-semibold mb-4">Features</h3>
              <ul className="space-y-2 text-gray-400">
                <li><a href="#" className="hover:text-blue-400 transition-colors">PDF Compression</a></li>
                <li><a href="#" className="hover:text-blue-400 transition-colors">Image Compression</a></li>
                <li><a href="#" className="hover:text-blue-400 transition-colors">Batch Processing</a></li>
                <li><a href="#" className="hover:text-blue-400 transition-colors">Quality Control</a></li>
              </ul>
            </div>
            <div>
              <h3 className="text-lg font-semibold mb-4">Resources</h3>
              <ul className="space-y-2 text-gray-400">
                <li><a href="#" className="hover:text-blue-400 transition-colors">Help Center</a></li>
                <li><a href="#" className="hover:text-blue-400 transition-colors">Blog</a></li>
                <li><a href="#" className="hover:text-blue-400 transition-colors">Tutorials</a></li>
                <li><a href="#" className="hover:text-blue-400 transition-colors">API Documentation</a></li>
              </ul>
            </div>
            <div>
              <h3 className="text-lg font-semibold mb-4">Contact</h3>
              <ul className="space-y-2 text-gray-400">
                <li>support@compressify.com</li>
                <li>+1 (555) 123-4567</li>
                <li>123 Compression Street</li>
                <li>Digital City, DC 12345</li>
              </ul>
            </div>
          </div>
          <div className="border-t border-gray-700 mt-8 pt-8 text-center text-gray-400">
            <p>&copy; 2023 Compressify. All rights reserved.</p>
          </div>
        </div>
      </footer>
    </div>
  );
};
export default PDFImageCompressor;
