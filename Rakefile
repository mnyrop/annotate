require 'fileutils'
require 'json'

task :default => [:store_annotations]

task :store_annotations do
  manifests = []
  Dir['annotations/*/'].each { | m | manifests << File.basename(m, ".*") }

  manifests.each do | manifest |
    unstored_canvases = Dir["annotations/" + manifest + "/*.json"].sort!
    unstored_canvases.each do |canvas|
      name = File.basename(canvas, ".*")
      dir = "annotations/" + manifest + "/" + name
      sum_annotations = JSON.parse(File.read(canvas))

      FileUtils::mkdir_p dir # make dir for canvas annotations

      make_anno_list(dir,name,manifest) # write canvas annotation list to file
      store_anno_array(dir,name,manifest,sum_annotations) # write array of canvas annotations to file

      File.delete(canvas) # remove unstored data file
    end

    unless unstored_canvases.empty?
      update_manifest_copy(manifest)
    end
  end
end



def make_anno_list(dir,name,manifest)
  listpath = dir + "/" + "list.json"
  if !File.exist?(listpath) # make annotation list if necessary
    puts "creating " + listpath + ".\n"
    File.open(listpath, 'w') do |f|
      # use manifest + '/' + name as canvas label, to ensure uniqueness across all manifests
      f.write("---\nlayout: null\ncanvas: '" + manifest + '/' + name + "'\n---\n" + '{% assign anno_name = page.canvas | append: "-resources" %}{% assign annotations = site.pages | where: "label", anno_name | first %}{"@context": "http://iiif.io/api/presentation/2/context.json","@id": "{{ site.url }}{{ site.baseurl }}/annotations/' + manifest + '/' + name + '/list.json","@type": "sc:AnnotationList","resources": {{ annotations.content }} }')
    end
  end
end

def store_anno_array(dir,name,manifest,sum_annotations)
  annopath = dir +  "/" + name + ".json"
  if !File.exist?(annopath) # if no preexisting annotation file
    puts "creating " + annopath + ".\n"
  else # if preexisting annotation file
    puts "appending new annotations to " + annopath + ".\n"
    old_annotations = JSON.parse(File.read(annopath).gsub(/\A---(.|\n)*?---/, ""))
    sum_annotations = sum_annotations.concat old_annotations # add annotation JSON to array
  end
  File.open(annopath, 'w') { |f| f.write("---\nlayout: null\nlabel: " + manifest + '/' + name + "-resources\n---\n" + sum_annotations.to_json) }
end

def update_manifest_copy(manifest)
  stored_canvases = []
  Dir['annotations/' + manifest + "/*/"].each { | c | stored_canvases << File.basename(c, ".*") }

  puts "adding annotation references for canvases " + manifest + '/' + stored_canvases.to_s + " to manifest copy."

  manifest_json = JSON.parse(File.read("iiif/" + manifest + "/clean-manifest.json").gsub(/\A---(.|\n)*?---/, "").to_s)
  canvases = manifest_json["sequences"][0]["canvases"].select {|c| stored_canvases.include? c["@id"].split('/')[-1] }

  canvases.each do | canvas |
    annotation_hash = Hash.new { |hash, key| hash[key] = {} }
    this_id = canvas["@id"].split('/')[-1]
    annotation_hash["@id"] = "{{ site.url }}{{ site.baseurl }}/annotations/" + manifest + "/" + this_id + "/list.json"
    annotation_hash["@type"] = "sc:AnnotationList"
    canvas["otherContent"] = Array.new << annotation_hash
  end

  File.open("iiif/" + manifest + "/manifest.json", 'w+') { |f| f.write("---\nlayout: null\n---\n"+manifest_json.to_json) }
end
