require 'asciidoctor'
require 'java'
require '/usr/share/java/batik-all.jar'
require '/usr/share/java/commons-io.jar'
require '/usr/share/java/commons-logging.jar'
require '/usr/share/java/xmlgraphics-commons.jar'
require '/usr/share/java/fop.jar'
require 'nokogiri'

task default: %w[document.pdf]

file 'document.pdf' => %w[index.fo fop.xconf] do |t|
  conf = Java::JavaIo::File::new 'fop.xconf'
  fo = Java::JavaIo::File::new 'index.fo'
  fop_factory = Java::OrgApacheFopApps::FopFactory::newInstance conf.getAbsoluteFile
  f = Java::JavaIo::File::new t.name
  fos = Java::JavaIo::FileOutputStream::new f
  out = Java::JavaIo::BufferedOutputStream::new fos
  fop = fop_factory.newFop Java::OrgApacheFopApps::MimeConstants::MIME_PDF, out
  xformer_factory = Java::JavaxXmlTransform::TransformerFactory::newInstance
  xformer = xformer_factory.newTransformer
  src = Java::JavaxXmlTransform::StreamSource::new fo
  res = Java::JavaxXmlTransformSax::SAXResult fop.getDefaultHandler
  xformer.transform src, res
end

file 'index.fo' => %w[index.xml] do |t|
  xml = Nokogiri::XML(File.read 'index.xml')
  xslt = Nokogiri::XSLT(File.read 'custom.xsl')
  fo = xslt.transform xml
  File.write t.name, fo
end

file 'index.xml' => %w[index.adoc] do |t|
  Asciidoctor.convert_file 'index.adoc', backend: 'docbook5', to_file: t.name
end
