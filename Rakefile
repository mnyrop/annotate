require 'fileutils'
require 'json'

task :store_annotations do
  data = Dir["annotations/*.json"].sort!
  data.each do |new_data|

    @name = File.basename(new_data, ".*")
    @dir = "annotations/" + @name

    FileUtils::mkdir_p @dir # make dir named after d

    listpath = @dir + "/" + "list.json" # make annotation list if necessary
    if !File.exist?(listpath)
      puts "creating " + listpath + ".\n"
      File.open(listpath, 'w') do |f|
        f.write("---\nlayout: null\ncanvas: " + @name + "\n---\n" + '{% assign anno_name = page.canvas | append: "-resources" %}{% assign annotations = site.pages | where: "label", anno_name | first %}{"@context": "http://iiif.io/api/presentation/2/context.json","@id": "{{ site.url }}{{ site.baseurl }}/annotations/' + @name + '/list.json","@type": "sc:AnnotationList","resources": {{ annotations.content }} }')
      end
    end

    annopath = @dir + "/" + @name + ".json"
    anno_file = File.read(new_data)
    annos = JSON.parse(anno_file)

    if !File.exist?(annopath) # if no preexisting annotation file
      puts "creating " + annopath + ".\n"
    else # if preexisting annotation file
      puts "appending new annotations to " + annopath + ".\n"
      old_file = File.read(annopath).gsub(/\A---(.|\n)*?---/, "").to_s
      annos = annos.concat JSON.parse(old_file)
    end

    File.open(annopath, 'w') do |f|
      f.write("---\nlayout: null\nlabel: " + @name + "-resources\n---\n" + annos.to_json)
    end

    File.delete(new_data) # remove data file
  end
end

task :write_manifest do
  canvas_ids = []
  Dir['annotations/*/'].each { | c_dir | canvas_ids << File.basename(c_dir, ".*") }
  puts "adding annotation references for canvases " + canvas_ids.to_s + " to manifest copy."

  m = File.read('build/clean-manifest.json').gsub(/\A---(.|\n)*?---/, "").to_s
  manifest = JSON.parse(m)

  manifest["sequences"][0]["canvases"].select {|c| canvas_ids.include? c["@id"].split('/')[-1] }.each do | canvas |
    this_id = canvas["@id"].split('/')[-1]
    anno_hash = Hash.new { |hash, key| hash[key] = {} }
    anno_hash["@id"] = "{{ site.url }}{{ site.baseurl }}/annotations/" + this_id + "/list.json"
    anno_hash["@type"] = "sc:AnnotationList"
    canvas["otherContent"] = anno_hash
  end

  # write new manifest to file
  File.open("manifest.json", 'w+') { |f| f.write("---\nlayout: null\n---\n"+manifest.to_json) }

end

task :store_and_reference => :store_annotations do
  Rake::Task["write_manifest"].invoke
end

task :default => [:store_and_reference]
