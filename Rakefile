require 'asciidoctor'
require 'java'
require '/usr/share/java/xmlgraphics-commons.jar'
require '/usr/share/java/fop.jar'
require 'nokogiri'

task default: %w[document.pdf]

file 'document.pdf' => %w[index.fo] do |t|
  fop_factory = Java::OrgApacheFopApps::FopFactory.newInstance;
  f = java.io.File.new(t.name)
  fos = java.io.FileOutputStream.new(f)
  out = java.io.BufferedOutputStream.new(fos)
  fop = fop_factory.newFop(org.apache.fop.apps.MimeConstants.MIME_PDF, out)
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
