require 'asciidoctor'
require 'java'
require '/usr/share/java/commons-logging.jar'
require '/usr/share/java/xmlgraphics-commons.jar'
require '/usr/share/java/fop.jar'
require 'nokogiri'

task default: %w[document.pdf]

file 'document.pdf' => %w[index.fo fop.xconf] do |t|
  conf = Java::JavaIo::File 'fop.xconf'
  fop_factory = Java::OrgApacheFopApps::FopFactory::newInstance conf;
  f = Java::JavaIo::File::new(t.name)
  fos = Java::JavaIo::FileOutputStream::new(f)
  out = Java::JavaIo::BufferedOutputStream::new(fos)
  fop = fop_factory.newFop Java::OrgApacheFopApps::MimeConstants::MIME_PDF, out
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
