WORKSPACE="asakusasatellite.xcworkspace"
SCHEME = "AsakusaSatellite"
PROVISIONING_PROFILE_NAME = ENV['PROVISIONING_PROFILE_NAME'] || "XC: org.codefirst.asakusasatellite"
PROVISIONING_PROFILE_PATH = ENV['PROVISIONING_PROFILE_PATH'] || "AppStore.mobileprovision"
TMP = "tmp"
ARCHIVE = "$(pwd)/#{TMP}/#{SCHEME}"
IPA = "#{ARCHIVE}.ipa"
PRETTY = (%x(which xcpretty); $?) == 0 ? "xcpretty -c" : "cat"
ALTOOL = "$(xcode-select -p)/../Applications/Application\\ Loader.app/Contents/Frameworks/ITunesSoftwareService.framework/Support/altool"

class String
    def bold; "\033[1m#{self}\033[22m" end
end

task :clean do
    puts "🚮  Cleaning...".bold
    sh "rm -rf #{TMP} && mkdir #{TMP}"
end

task :archive => :clean do
    puts "🔨  Archiving...".bold
    sh "xcodebuild archive -workspace #{WORKSPACE} -scheme #{SCHEME} -archivePath #{ARCHIVE} | #{PRETTY}"
end

task :ipa => :archive do
    puts "📦  Creating ipa...".bold
    # sh "xcrun -sdk iphoneos PackageApplication #{ARCHIVE}.xcarchive/Products/Applications/#{SCHEME}.app -o #{IPA} -embed #{PROVISIONING_PROFILE_PATH}"
    # exportArchiveだとarchived-expanded-entitlements.xcentがうまく処理されない??
    sh "xcodebuild -exportArchive -archivePath #{ARCHIVE}.xcarchive -exportPath #{ARCHIVE} -exportFormat ipa -exportProvisioningProfile '#{PROVISIONING_PROFILE_NAME}' | #{PRETTY} && exit ${PIPESTATUS[0]}"
end

task :submit => :ipa do
    puts "🔌  Contacting to iTunes Connect...".bold

    require 'io/console'
    print "iTunes Connect ID or Email: "
    user = STDIN.gets.strip
    print "iTunes Connect Password: "
    password = STDIN.noecho(&:gets).strip
    puts ""

    puts "✈️  Submitting to TestFlight...".bold
    sh "#{ALTOOL} --upload-app --file #{IPA} --username #{user} --password #{password}", verbose: false

    puts "🎉  Submitted!".bold
end

