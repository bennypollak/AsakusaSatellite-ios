platform :ios, '8.0'
use_frameworks!


target 'AsakusaSatellite' do
  pod 'AsakusaSatellite'
  pod 'NorthLayout'
  pod 'HanekeSwift'
  pod 'TUSafariActivity', '~> 1.0'
  pod 'Fabric'
  pod 'Crashlytics'


  target 'AsakusaSatelliteTests' do
    inherit! :search_paths
    pod 'Quick'
    pod 'Nimble'
  end
end

target 'ShareExtension' do
  pod 'AsakusaSatellite'
end

plugin 'cocoapods-app_group', targets: ['ShareExtension']
