require 'fileutils'
require 'json'

task :store_annotations do
  manifests = []
  Dir['annotations/*/'].each { | m | manifests << File.basename(m, ".*") }

  manifests.each do | manifest |
    unstored_canvases = Dir["annotations/" + manifest + "/*.json"].sort!
    unstored_canvases.each do |canvas|

      @name = File.basename(canvas, ".*")
      @dir = "annotations/" + manifest + "/" + @name
      @sum_annotations = []

      listpath = @dir + "/" + "list.json"
      annopath = @dir +  "/" + @name + ".json"

      new_annotations = JSON.parse(File.read(canvas))

      FileUtils::mkdir_p @dir # make dir for canvas annotations

      if !File.exist?(listpath) # make annotation list if necessary
        puts "creating " + listpath + ".\n"
        File.open(listpath, 'w') do |f|
          f.write("---\nlayout: null\ncanvas: " + @name + "\n---\n" + '{% assign anno_name = page.canvas | append: "-resources" %}{% assign annotations = site.pages | where: "label", anno_name | first %}{"@context": "http://iiif.io/api/presentation/2/context.json","@id": "{{ site.url }}{{ site.baseurl }}/annotations/' + manifest + '/' + @name + '/list.json","@type": "sc:AnnotationList","resources": {{ annotations.content }} }')
        end
      end

      if !File.exist?(annopath) # if no preexisting annotation file
        puts "creating " + annopath + ".\n"
      else # if preexisting annotation file
        puts "appending new annotations to " + annopath + ".\n"
        old_annotations = JSON.parse(File.read(annopath).gsub(/\A---(.|\n)*?---/, ""))
        @sum_annotations = new_annotations.concat old_annotations # add annotation JSON to array
        puts @sum_annotations
      end

      File.open(annopath, 'w') { |f| f.write("---\nlayout: null\nlabel: " + @name + "-resources\n---\n" + @sum_annotations.to_json) }
      # File.delete(canvas) # remove unstored data file
    end

    stored_canvases = []
    Dir['annotations/' + manifest + "/*/"].each { | c | stored_canvases << File.basename(c, ".*") }
    puts "adding annotation references for canvases " + stored_canvases.to_s + " to manifest copy."

    manifest_json = JSON.parse(File.read("iiif/" + manifest + "/clean-manifest.json").gsub(/\A---(.|\n)*?---/, "").to_s)

    manifest_json["sequences"][0]["canvases"].select {|c| stored_canvases.include? c["@id"].split('/')[-1] }.each do | canvas |
      empty_array = Array.new
      annotation_hash = Hash.new { |hash, key| hash[key] = {} }
      this_id = canvas["@id"].split('/')[-1]
      annotation_hash["@id"] = "{{ site.url }}{{ site.baseurl }}/annotations/" + manifest + "/" + this_id + "/list.json"
      annotation_hash["@type"] = "sc:AnnotationList"
      canvas["otherContent"] = empty_array << annotation_hash
    end

    # write new manifest to file
    File.open("iiif/" + manifest + "/manifest.json", 'w+') { |f| f.write("---\nlayout: null\n---\n"+manifest_json.to_json) }

  end
end

task :default => [:store_annotations]
