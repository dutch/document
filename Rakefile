require 'asciidoctor'

task default: %w[index.xml]

file 'index.xml' => %w[index.adoc] do |t|
  Asciidoctor.convert_file 'index.adoc', backend: 'docbook5', to_file: t.name
end
