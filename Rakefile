require 'asciidoctor'

task default: %w[index.fo]

file 'index.fo' => %w[index.xml] do |t|
  xml = Nokogiri::XML(File.read 'index.xml')
  xslt = Nokogiri::XSLT(File.read 'custom.xsl')
  fo = xslt.transform xml
  File.write t.name, fo
end

file 'index.xml' => %w[index.adoc] do |t|
  Asciidoctor.convert_file 'index.adoc', backend: 'docbook5', to_file: t.name
end
